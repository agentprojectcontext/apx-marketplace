# Android TV extension

Optional APX extension: **APK + scoped HTTPS connection + MCP tools + agent skill**. No TV-specific methods, tools or routes are added to APX core. This example uses APX's currently supported extension manifest, not the marketplace's proposed v2 manifest.

## Included

- `android-tv.apk`: Android TV companion; native Android Accessibility D-pad/OK, app launch, media volume, semantic screen reading and screenshot fallback.
- Dependency-free Node 24 MCP server: `tv_pairing`, `tv_companion`, `tv_status`, `tv_image`, `tv_playback`, `tv_open`, `tv_buttons`, `tv_search`, `tv_volume`.
- `skills/android-tv/SKILL.md`: installation, safe operation, recovery and removal. TV requests use this skill and these tools, not a general phone-control skill.
- Source under `companions/android-tv` in the repository, plus behavioral tests. The bundled APK is a development build, not a production-signed public release.

**Control does not use ADB.** Platform-tools are an optional one-time installation/configuration aid. The server has no ADB subprocess or arbitrary shell capability. Android Accessibility supplies D-pad actions even when an app exposes no readable nodes; that does not guarantee the agent can see or safely navigate every app.

## Requirements

- APX extension/MCP support and Node 24 on the core machine.
- Android TV 13+ and permission to install the companion APK. Enable APX TV's Accessibility service.
- A trusted HTTPS route to this extension's loopback listener, separate from APX's panel and account token. The tested setup uses Tailscale on both devices, with **Serve, not Funnel**, for the extension.
- Existing HBO/Netflix account and entitlement. Credentials, PINs, payment and subscription dialogs are handled by the owner, never this extension.

## Install and connect

1. Install the extension using `apx extensions inspect <folder>`, `apx extensions install <folder>`, then review its code permission before enabling it.
2. Configure `connection_key`, a random 32-byte hex value, through APX's protected extension-value store. For example, pipe `node -e 'process.stdout.write(require("node:crypto").randomBytes(32).toString("hex"))'` directly into `apx extensions secret android_tv connection_key`. Do not print, commit or reuse the key, and never use an APX account token.
3. Choose a private HTTPS origin for this extension and save it as the protected `address` value. The MCP server binds **127.0.0.1:18745**. With Tailscale, an example private mapping is `tailscale serve --bg --https=8444 http://127.0.0.1:18745`; use the actual HTTPS origin reported by Tailscale, including port. Do not enable Funnel, replace unrelated mappings or proxy arbitrary core routes.
4. Turn on the extension with `apx extensions on android_tv`. Call `tv_companion` to start/check its listener. It initially reports disconnected until the TV pairs.
5. Install `android-tv.apk` on the TV. If using platform-tools, pair modern authenticated wireless debugging with the owner present, connect the displayed **connection** port, and run `adb -s <paired-device> install -r android-tv.apk`. Pairing and connection ports can differ. Never enable unauthenticated port 5555 or expose debugging to the internet.
6. Open APX TV. Run `tv_pairing` and enter its HTTPS address and six-digit, one-use code in the APK. The code lasts five minutes and permits five attempts. It grants access only to this extension. The APK stores its connection credential encrypted using Android Keystore. There is no general APX token in the APK.
7. Enable **TV control settings** / APX TV Accessibility. Optionally allow **playback status** notification access to read HBO/Netflix/YouTube media sessions. The listener does not read general notification content. Without this permission, report title/state metadata unavailable.
8. Optional wake setup: some TVs require **Turn screen on** access. During authorized installation only, `adb -s <paired-device> shell cmd appops set app.apx.tv TURN_SCREEN_ON allow` grants that permission. It persists independently of debugging. Android wake is not proof that an HDMI-connected television panel is on; HDMI-CEC and deep-sleep behavior vary.
9. Verify `tv_companion`, `tv_status`, app opening and a safe navigation action through the MCP tools. For a finished end-user installation, **disable both wireless and USB debugging**, disconnect USB, and verify the same tools still work. Keep debugging enabled only when explicitly requested for development, and disclose that the physical debugger-off test has not been performed on that TV.

For development, `scripts/setup-tv-development.mjs` assists only with explicitly authorized APK installation/configuration. `--allow-wake` enables wake access; `--allow-playback-status` enables the optional listener. Inspect the script before use. It is not part of the extension's runtime transport.

## Operation and limits

- Launching an app or delivering a key is not playback proof. Use `tv_playback` for exact title/episode, PLAYING and advancing position. Do not interrupt an already correct program merely to test.
- Prefer semantic labels. If absent, inspect `tv_image`; send `select` alone only after verifying focus. Images expire after 30 seconds and selections consume their observation. The APK checks supported apps and recognized private/purchase labels, but opaque content can hide them. This is **visual assistance, not a mechanical guarantee** that a login or purchase is impossible.
- `tv_open` also accepts a Netflix-only `titleId` (6–12 digits) for a direct watch link. Verify the ID against the exact requested title on Netflix's official catalog and have the owner confirm the intended active profile first. The extension constructs a fixed HTTPS Netflix watch URL and restricts its target package; it does not accept arbitrary URLs, launch other apps, select profiles or bypass PINs. This is experimental: the tested Netflix app can stop at its profile selector or return home without opening the requested title. Do not advertise unattended Netflix playback from successful intent delivery. That app also did not declare `MEDIA_PLAY_FROM_SEARCH`; do not assume Google Assistant's private integration is a public playback API.
- Semantic status includes bounded labels/focus and recent accessibility events, without screenshots. Apps may not expose any; no text is invented from an opaque/black frame.
- Android 14+ tries per-window screenshots with a public display-capture fallback; earlier supported Android versions use display capture. Both obey Android's protected-content restrictions. Secure/protected surfaces can be black or unavailable, including navigation UI in some apps. A black image never authorizes selecting blindly. Owner assistance may be necessary even though native remote buttons can be delivered.
- `tv_open` accepts `app: "google"` with a short `query` to request Google TV title results through Android's global-search activity in the installed Google TV search package. It does not simulate speech or use a private Assistant API. Inspect results before selecting a streaming service; stop for purchases, rentals, PINs or login. Google catalog availability and deep links are not playback proof. This expands screen observation/navigation to Google TV search and its Google TV launcher (which can display the resulting title cards), not arbitrary apps. TalkBack is not required: enable APX TV's Accessibility service, not the screen reader.
- YouTube: verify the official channel and current broadcast on the web, then call `tv_open` with `app: "youtube"` and an eleven-character `videoId`. The APK constructs the fixed YouTube HTTPS watch address and restricts it to the TV YouTube package. No arbitrary URLs, account changes or subscriptions are accepted. Check PLAYING, title and advancing position in `tv_playback`; a title containing "live" alone does not prove a current live broadcast. Refresh the channel's live page rather than caching a changing video ID.
- The APK uses the existing APX Android app's logo and dark neutral visual tokens, with a TV-sized layout and green D-pad focus ring. Pairing fields support explicit up/down navigation; code entry remains masked and disables screen capture for the rest of that screen session once entered, so clearing a code cannot expose an earlier rendered frame. English and Spanish are included; narrow layouts stack rather than squeeze the form.
- The skill stays generic. Verified owner-specific routes belong in APX private memory, not the published skill or extension files. The agent uses APX's existing memory tools; no plugin database or general account token is needed. Global private memory is distinct from a project named `default`. Remember a destination only as far as the evidence establishes, and revalidate it before repeating.
- Netflix PIN/password entry from APX Variables is **not implemented**. A test marker successfully typed elsewhere would not establish safe PIN delivery. The tested Netflix app exposes neither semantic fields nor an observable capture, so the extension cannot bind a secret to a verified input field. Do not bypass protected screens or enter even a guessed PIN blindly. Authentication remains owner-operated; a future secret-input feature needs explicit app/field binding and no value in model-visible arguments, observations or logs.
- `tv_search` supports recognized focused native search fields. Custom TV keyboards require visible D-pad navigation and per-selection inspection. There is no ADB text-injection fallback.
- `tv_volume` reports actual Android media-stream steps. A requested 2% can become the lowest non-zero step, for example 1/25 = 4%. Fixed HDMI output is reported as not adjusted. Android stream level does not prove the external television speaker level.
- Commands are serialized and deadline-limited, with bounded queues and identifiers. Cancelled or timed-out commands may already have been delivered: inspect state before any retry. There are no automatic effect retries.
- The APK reconnects after temporary network/server failures while its foreground service is running. Network availability and device background-service policy still matter. Reopen APX TV if Android stopped it; ensure the VPN/network app is connected.

## Disconnect and revoke

Use **Disconnect and forget** on the APK, then disable the extension. To revoke a copied connection credential, replace the extension connection key, restart the extension and pair again. Requesting another pairing code does **not** revoke an existing connection credential. Disable Accessibility/notification access or uninstall the APK to remove its Android privileges. Remove only the extension's own HTTPS mapping if no longer needed.

## Verification

```sh
node --test examples/android-tv/*.test.mjs
apx extensions inspect "$(pwd)/examples/android-tv"
pnpm test
node src/cli.ts validate
npx tsc --noEmit
```

APK build requires Android SDK 35, JDK 17 and Gradle (wrapper included):

```sh
companions/android-tv/gradlew -p companions/android-tv assembleDebug lintDebug
APX_TV_APK=companions/android-tv/app/build/outputs/apk/debug/app-debug.apk \
  AAPT="$ANDROID_HOME/build-tools/35.0.0/aapt" \
  node --test companions/android-tv/tests/feedback.test.mjs
```

The companion declares generic accessibility feedback only. It does not provide spoken output and must not identify itself as a screen reader. Android apps can query enabled services by feedback type using `AccessibilityManager.getEnabledAccessibilityServiceList(FEEDBACK_SPOKEN)`; declaring spoken feedback can therefore affect another app even when the companion has no text-to-speech engine. The packaged-resource regression test rejects that declaration. This establishes the platform detection mechanism, not which detection logic a particular Netflix or YouTube version uses.

To update an existing development installation, use `adb -s <paired-device> install -r android-tv.apk` with the same signing key. Do not uninstall, clear app data, or rerun initial development setup: those operations can discard or replace pairing. Reopen APX TV if Android stops it during replacement, then verify `tv_companion` and `tv_status` through the HTTPS tools. If wireless debugging has no active connection, obtain its current authenticated connection endpoint before installation; the APK control channel cannot install its own replacement.

Hardware evidence must distinguish this APK transport from earlier ADB experiments. Real APK/HTTPS checks have exercised pairing, semantic reading of the companion, waking Android, HBO launch and screenshot, Netflix launch, and media volume readback (requested 2%, actual 1/25 = 4%). Google TV title-card navigation and provider selection have been inspected through the APK; catalog editions must still be checked. An official YouTube live broadcast was opened by the APK with PLAYING and increasing media-session position verified. Generic D-pad/OK behavior has also been checked in an Android TV emulator. The platform-only `UiProbe` instrumentation runner tests pairing focus order, invalid-address feedback, retry, and secure capture flags with a fictitious code in English and Spanish; it refuses physical devices. It adds no test dependency. These UI tests reproduced and fixed a native field consuming the remote's down arrow. Filled-code screenshots are deliberately protected; error captures clear the code before capture. These facts do not establish Netflix movie playback, universal app support, physical TV power, or debugger-off operation on a development device whose owner requested that debugging stay on.

Docs impact: updated — this README and the bundled skill document APK permissions, scoped pairing, private HTTPS setup, volume granularity, protected-screen limitations, debugging removal, revocation and recovery. No APX core API or core documentation changes are required.
