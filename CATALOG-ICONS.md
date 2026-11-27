# Catalog brand artwork

The official catalog identifies each connected service with its recognizable brand artwork whenever usable artwork is available. Prefer an official press/brand kit, then Simple Icons with its published brand color, then an official site icon. The artwork license does not transfer trademark rights.

Do not recolor, invert, distort, rotate or crop the mark. Preserve its aspect ratio and clear space. Place it on a neutral rounded carrier so marks remain visible in dark and light Marketplace themes: white for dark or colored marks, dark for official white artwork (Canvas LMS). Empty source-image padding can be normalized; do not crop the mark itself. The carrier is not a new brand lockup. Use the official brand reference linked in each package for future updates.

Each package has `assets/ICON-PROVENANCE.md`: retrieval/review date, exact source URL, brand guidance or artwork license, presentation and packaged icon SHA-256. `catalog-icons.json` is the machine-readable audit inventory. Simple Icons artwork is pinned to version 16.38.0, CC0-1.0; no new runtime dependency is added. Official kit and site artwork retains its owner's rights. Attribution and affiliation notices belong in provenance, not in visible catalog descriptions.

## Exceptions reviewed on 2026-11-18

| Extension | Why its existing symbol remains |
| --- | --- |
| Apple Calendar | Apple's published trademark guidelines, Unauthorized Use §2, require a written trademark license for Apple-owned icons. None is recorded for this package. |
| Apple Contacts | Same Apple restriction. |
| Apple Mail | Same Apple restriction. |
| Apple Notes | Same Apple restriction. |
| Apple Reminders | Same Apple restriction. |
| DeepWiki | Direct official-icon downloads return HTTP 429. The owner requested retaining the existing symbol instead of further retries. |

Apple reference: <https://www.apple.com/legal/intellectual-property/guidelinesfor3rdparties.html>.
DeepWiki attempted asset: <https://deepwiki.com/apple-icon.png?a931a96537bdf6db>.

Terraform Registry 0.1.1 uses an APX-made neutral **T**, with no HashiCorp artwork in the package or listing. The exception applies only to this extension; the listing links to the official Terraform Registry.

Terraform Registry 0.1.1 usa una **T** neutra creada por APX, sin logo de HashiCorp en el paquete ni en la ficha. La excepción aplica solo a esta extensión; la ficha enlaza al registro oficial de Terraform.

Android TV uses the officially downloadable Android robot, not the restricted Android TV wordmark. The Android robot is reproduced from work created and shared by Google and used according to terms described in the [Creative Commons 3.0 Attribution License](https://creativecommons.org/licenses/by/3.0/). Android is a trademark of Google LLC.

## Publishing artwork changes

Artwork and provenance inside an extension change its package fingerprint. Increment the package version, register the committed source, and preserve every previous version in the registry and public index. Never replace historical archives with newly packed bytes. Regenerate the distribution, verify the exact public commit with `scripts/verify-public.mjs`, and compare downloaded files and archive SHA-256 values against the served index.

Verify the live Marketplace at desktop and phone widths in both themes. These catalog-only changes do not modify the APX application or its owner core.

Docs impact: updated — this policy, the machine-readable audit inventory and every package's icon provenance replace the previous blanket preference for generic service symbols. Integration setup, credentials, permissions and runtime behavior are unchanged.
