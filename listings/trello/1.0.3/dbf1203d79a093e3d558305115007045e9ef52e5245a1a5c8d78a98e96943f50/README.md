# Trello

Version 1.0.1: [request cancellation and deadline / cancelación y límite de solicitudes](RUNTIME.md). Reinstall to update an installed copy; approval is not inherited automatically.

Optional, read-only APX extension: boards, lists and cards. No native core change and no additional agent or routine. Requires Node.js 24+ and a Trello account whose policies allow API access. **Live account access is unverified.**

## Install and connect

```bash
apx extensions inspect "$PWD/integrations/trello"
apx extensions install "$PWD/integrations/trello"
```

It installs **off**. In Resources > Extensions > Trello, the owner enters `api_key` and `api_token`, reviews the files and enables it. Never put credentials in chat, command arguments, source files or a public variant. APX resolves the two extension-scoped secret references only into the server process environment.

Create an API key through [Trello Power-Up administration](https://trello.com/apps/admin). Follow [Trello authorization](https://developer.atlassian.com/cloud/trello/guides/rest-api/authorization/) in your own browser to generate a user token, selecting **read only**, preferably a one-day expiry. Do not grant write or account/email scope. The documented authorization URL uses the API key to begin consent; this server sends the key/token in an Authorization header, never its request URL. Browser consent does not automatically import a token into APX: copy it into the extension's secret form yourself. There is no OAuth callback or refresh flow in this version. Replace expired/revoked tokens in the form. Revoke under Trello account settings > Applications; disable/remove the extension too.

## Use and limits

- `trello_boards`: open boards by default; optionally closed/all.
- `trello_lists`, `trello_cards`: an observed 24-character board id; open/closed/all.
- `limit` 1–20 (default 10), `offset` 0–10000. Each call fetches a fresh full service list (at most 1 MiB) and returns a local slice with `next_offset`. This is **not server pagination or a stable snapshot**. Large responses fail explicitly instead of pretending to have listed everything. Cards return only id/name/description/url/list/due/completion/closed fields. No comments, attachments, member emails, mutations or sharing.
- Fixed `https://api.trello.com/1` origin, redirects refused, 20-second request budget, no automatic retry, bounded untrusted output. Both credentials are redacted before truncation. Account permissions still determine accessible boards; the read-only tool set does not reduce the provider-side authority of an accidentally over-scoped token.
- Trello receives the account's queries. Returned board text enters agent context as untrusted data. API availability, plan limits and provider pricing apply; APX includes no Trello subscription or quota.

## Verification and provenance

Run `node --test tests/trello.test.mjs` from the repository root. Tests cover fixed destinations/header auth, all three read contracts, paging limits, credential/URL injection, dual-secret redaction, provider errors, and the real stdio process. These are scripted vendor responses, not a live account verification.

Independent implementation from [official authorization documentation](https://developer.atlassian.com/cloud/trello/guides/rest-api/authorization/) and [official API routes](https://developer.atlassian.com/cloud/trello/rest/api-group-boards/), including the published OpenAPI `1.1274.0`. The independently written package is MIT licensed; see `LICENSE`. No code copied from PolyForm-Noncommercial sources. Trello's service terms apply separately. The capability inventory retains references to openbot/openclaw/openmausbot without treating their availability claims as validation.

## Español

Extensión opcional de solo lectura: tableros, listas y tarjetas. Se instala **apagada**. En Recursos > Extensiones > Trello, ingresá tu clave y un token **solo lectura** obtenido en tu navegador, preferentemente con vencimiento de un día; no los pegues en el chat ni concedas permisos de escritura o cuenta/email. Los datos van por un encabezado a Trello, no por la URL de las consultas. Reemplazá el token vencido en el formulario; no hay renovación automática. Para revocarlo, usá Aplicaciones en la configuración de tu cuenta de Trello y apagá o quitá la extensión.

Cada consulta obtiene una lista completa del servicio, limitada a 1 MiB, y devuelve hasta 20 elementos mediante un desplazamiento local. No es paginación del servidor ni una foto estable: la lista puede cambiar entre consultas. Los textos recibidos no son instrucciones. Sin edición, comentarios, adjuntos, correos de miembros ni compartir. Se aplican las políticas, cuotas y costos de tu cuenta; APX no incluye una suscripción. **No se verificó una cuenta real.**

Docs impact: this README, bilingual listing, authoring skill and capability inventory describe the implemented subset and limits. No new dependency, native APX method, OAuth scope or approval policy.
