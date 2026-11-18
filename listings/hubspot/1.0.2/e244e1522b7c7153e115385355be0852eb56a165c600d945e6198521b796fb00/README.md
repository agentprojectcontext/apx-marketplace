# HubSpot

Version 1.0.1: [request cancellation and deadline / cancelación y límite de solicitudes](RUNTIME.md). Reinstall to update an installed copy; approval is not inherited automatically.

Optional, single-account, read-only CRM adapter. Two tools list and read active contacts, companies, deals and notes using your own account's static app token. No additional agent, routine, dependency, native APX change or hosted OAuth service. **Live account access is unverified.**

## Install and connect

```bash
apx extensions inspect "$PWD/integrations/hubspot"
apx extensions install "$PWD/integrations/hubspot"
```

It installs **off**. In Resources > Extensions > HubSpot, the owner enters `api_token`, reviews the files and enables it. APX resolves `secret://api_token` only into the server process environment. Never paste credentials into chat, arguments, a public variant or source files.

For new accounts, use the [current developer platform authentication flow](https://developers.hubspot.com/docs/apps/developer-platform/build-apps/authentication/overview): create your own app with private distribution, static auth and read-only scopes, install it in your one authorized account, then copy its static token from app settings into APX. This requires the account's app-development/administrator permissions. Do not use a developer API key or CLI personal access key as the CRM token. There is no APX-managed HubSpot app or automatic registration. OAuth is required for a centrally distributed multi-account HubSpot app; this package is not that app and is not a HubSpot Marketplace listing.

**New legacy private apps cannot be created since October 26, 2026.** An existing legacy app token is still supported by HubSpot, but is not the onboarding path for a new account. See the [legacy app notice and rotation instructions](https://developers.hubspot.com/docs/apps/legacy-apps/private-apps/overview).

Request only the needed read scopes: `crm.objects.contacts.read`, `crm.objects.companies.read`, `crm.objects.deals.read`. Check the [endpoint scope requirements](https://developers.hubspot.com/docs/apps/developer-platform/build-apps/authentication/scopes) for notes and account-specific availability. Do not grant write, sensitive-data, marketing or email-send scopes to make a failing read work. If permitted notes access cannot be established, leave that capability unused and report the restriction. Read-only tools do not reduce the authority of an accidentally over-scoped token.

Static tokens have no refresh exchange here. Rotate/revoke them in HubSpot app settings, replace the value in the APX secret form and reconnect. Expired/revoked tokens or removed account permissions fail safely with no retry. Remove/disable the extension when no longer used. A short-lived OAuth access token is not a substitute for this static-token onboarding.

## Tools and boundaries

- `hubspot_list`: kind `contacts`, `companies`, `deals` or `notes`; limit 1–20 (default 10), optional `after` from `next_after`. One provider page, no automatic account scan. `paging.next.link` is ignored, never followed or exposed. Repeated, secret-bearing or invalid cursors are rejected.
- `hubspot_get`: the same kind and an observed numeric id. Active records only. Contacts expose first/last name, company and job title; companies name/domain/industry; deals name/stage/amount/close date/pipeline; notes body/timestamp. No email/phone expansion, archived records, custom/sensitive properties, associations, attachments or mutations. Null/missing values remain distinct; amounts have no inferred currency and stage ids are not fabricated labels.
- Fixed `https://api.hubapi.com/crm/v3/objects` routes, bearer header, no secret query parameters, redirects refused, 20-second request limit, no retries, 1 MiB response cap and bounded output. Returned fields/HTML are untrusted text, not instructions or executable HTML. Token echoes are redacted before string truncation.
- HubSpot receives account queries; selected private CRM data enters agent context. Your account's edition, API access, scopes and quotas apply. APX includes no HubSpot subscription. No background synchronization or commercial multi-account OAuth backend is provided.

HubSpot's [official remote MCP connector](https://developers.hubspot.com/docs/apps/developer-platform/build-apps/integrate-with-the-remote-hubspot-mcp-server) exists and has wider capabilities. It requires a pre-registered connector, client credentials, matching redirect and PKCE. APX's current extension manifest has no static OAuth client-registration fields; its usual dynamic-registration helper is not proof this flow works. No empty MCP listing or secret-in-arguments workaround is included.

## Verification and provenance

Independent MIT implementation, see `LICENSE`. The upstream reference is a Composio service-name declaration in OpenMausBot, **not vendored HubSpot functionality**. No upstream connector code copied. API routes were checked against official contacts/notes guides and HubSpot's public Notes v3 OpenAPI. Scripted tests and real stdio do not prove live authorization, scope availability or vendor parity.

## Español

Extensión opcional de solo lectura para **una cuenta propia**: contactos, empresas, negocios y notas activos, con campos acotados y páginas de hasta 20 registros. Se instala **apagada**. En Recursos > Extensiones > HubSpot, ingresá el token estático de tu app; nunca en el chat. Las apps privadas antiguas ya no se pueden crear: para una cuenta nueva usá la plataforma actual, distribución privada y autenticación estática, con permisos de lectura y autorización del administrador. Los tokens de apps antiguas existentes son una opción de compatibilidad, no el alta recomendada.

No es una app OAuth compartida entre clientes, no se publica en HubSpot Marketplace y no renueva tokens. Rotá o revocá el token en HubSpot y reemplazalo en el formulario de APX. Un token OAuth temporal no reemplaza esta configuración. La disponibilidad de notas depende de los permisos de tu cuenta: no concedas escritura ni acceso sensible para intentar resolver un rechazo.

Sin edición, envío de correo, datos archivados, adjuntos ni asociaciones. Los textos recibidos son datos no confiables; los importes no implican una moneda y los campos ausentes no se inventan. Se aplican el plan, las cuotas y los permisos de HubSpot. **No se verificó una cuenta real.**

Docs impact: package README/skill/bilingual listing and capability matrix describe the static-token subset and official-OAuth limitation. No APX protocol, native permission or dependency changed.
