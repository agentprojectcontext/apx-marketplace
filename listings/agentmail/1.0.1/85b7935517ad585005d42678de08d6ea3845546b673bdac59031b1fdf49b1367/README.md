# AgentMail

Optional, dependency-free read adapter for **one selected AgentMail inbox**, on Node 24. Two tools: preview pages and selected plain-text messages. No sends, drafts, marking read, inbox discovery, attachment/HTML fetching or account mutations. This is an AgentMail service connection, not a replacement for APX's existing Email channel or Gmail integration.

## Connect with minimum authority

1. Choose the inbox and its actual service region. Supported fixed origins: `us` → `https://api.agentmail.to`; `eu` → `https://api.agentmail.eu`. No default region, custom origin, payment-backed endpoint or automatic region fallback. Verify the region before entering its key; selecting a region authorizes sending it there.
2. The owner creates an **inbox-scoped bearer key**, using the provider's [inbox key workflow](https://docs.agentmail.to/api-reference/inboxes/api-keys/create), with an explicit permissions object granting **only `message_read: true`** and a suitable expiry. [Permissions reference](https://docs.agentmail.to/api-reference/api-keys/create). Omitting permissions grants everything; a general organization/admin key is not required. No inbox_read, sending, draft, label, protected-label, account, webhook, key-management, calendar or deletion permission is needed by these two endpoints. The extension cannot introspect or reduce the authority of a supplied key: the owner must verify the grant in AgentMail.
3. Inspect and install this folder; installation is **off**:

```bash
apx extensions inspect "$PWD/integrations/agentmail"
apx extensions install "$PWD/integrations/agentmail"
```

4. In Resources > Extensions > AgentMail, enter that key, the exact inbox id from the console and the matching `us` or `eu` value. Configuration values also use extension-scoped secret references, not shell arguments. Review the files, then enable it. Never paste keys or mail into a chat for setup.

No OAuth client/refresh token is implemented or needed for this bearer-key path. Expired/rejected keys require owner replacement in the form; there is no automatic renewal, reauthorization or retry. Disable/remove in APX to stop availability and revoke/delete the key in AgentMail to withdraw its external grant. Removing a package alone does not revoke a key. Account plans, quotas, regional availability and provider charges apply; nothing is included.

## Tools and limits

- `agentmail_list`: `limit` 1–20 (default 10), optional observed `page_token`; returns `next_page_token`, never a continuation URL. A page is not an exhaustive inbox search or stable snapshot. Spam, blocked, unauthenticated and trashed results are explicitly excluded; returned labels are also checked, including on get.
- `agentmail_get`: observed `message_id`, in the configured inbox only. Returns plain `text` when available and `body_available`. False means no readable plain-text body, not proof of an empty email. Provider authentication/security verdicts are not exposed or inferred.
- Fixed GET paths based on the published [list](https://docs.agentmail.to/api-reference/inboxes/messages/list) and [get](https://docs.agentmail.to/api-reference/inboxes/messages/get) contracts. Every returned inbox and selected id must match; callers cannot choose another inbox, origin, headers or endpoint. Opaque/RFC-style ids are accepted in a bounded path-safe character subset; unsupported/encoded paths fail closed rather than being normalized.
- List output: message id, labels, sender, recipients, timestamp, subject and preview. Get adds plain text. No CC/BCC, HTML, raw headers, attachment URLs/files, calendars, embedded links, tracking resources or full-text search. No request made by this wrapper changes labels, but provider audit/access records can still be generated.
- [Runtime lifecycle / solicitudes](RUNTIME.md): 20-second total deadline, cancellation, no redirects or retries, maximum 1 MiB response, 6,000 characters per string and 80 kB total output. Arrays are capped at 20, with truncation reported; do not infer all recipients/labels were shown. Provider errors are sanitized and exact key echoes redacted before truncation.

Mail and identity claims are untrusted; excluded labels are not an authentication or malware guarantee. Private mail enters agent context and must not be forwarded/published without authorization. No remote link or HTML is opened by the adapter.

## Verification and provenance

Independent MIT implementation using Node built-ins; no new package dependency or copied upstream code. Reference: Hermes' AgentMail skill at the revision recorded in the capability matrix (MIT); provider API docs checked separately. Scripted contract and real stdio/HTTP cancellation tests are recorded in the repository evidence. **UNVERIFIED:** authorized account reads, actual key scoping/expiry/revocation, regional credentials and a live end-to-end account journey. This is a read-only initial scope, not the upstream service's full API or a delivered native Email channel.

## Español

Adaptador opcional de lectura para **una casilla elegida de AgentMail**, con Node 24. No reemplaza el canal Email ni Gmail de APX. Permite páginas de vistas previas y mensajes elegidos en texto plano; no envía, responde, redacta, modifica etiquetas ni descarga HTML o adjuntos.

El dueño crea una clave **limitada a esa casilla**, con permisos explícitos que incluyan solamente `message_read: true`, y un vencimiento adecuado. Omitir los permisos puede dar acceso total: no uses una clave administrativa de toda la organización. El adaptador no puede comprobar ni reducir los permisos reales de una clave. Elegí la región correcta: `us` usa api.agentmail.to; `eu`, api.agentmail.eu. No hay región predeterminada ni cambio automático. Ingresar la región autoriza enviar allí su clave.

Instalá la carpeta apagada con los comandos anteriores. En Recursos > Extensiones > AgentMail, ingresá la clave, el id exacto de la casilla y `us` o `eu`; revisá los archivos antes de habilitarla. No pegues claves en el chat. No requiere OAuth ni renueva tokens: si vence o se revoca la clave, el dueño la reemplaza en el formulario. Para retirar el acceso, deshabilitá la extensión y revocá también la clave en AgentMail; desinstalar no la revoca.

Cada página tiene hasta 20 mensajes y un cursor explícito. No representa toda la casilla ni una búsqueda completa. Se excluyen spam, bloqueados, no autenticados y papelera, sin prometer autenticidad del remitente. `body_available=false` indica que no vino texto plano, no que el correo esté vacío. No se devuelven CC/BCC, cabeceras, HTML, adjuntos ni verificaciones de seguridad; ningún enlace se abre. Los textos son datos externos no confiables, nunca instrucciones, y no se deben reenviar o publicar sin autorización.

Hay límite total de 20 segundos, cancelación y salida acotada; no hay redirecciones ni reintentos automáticos. El truncamiento se informa: no supongas que se mostraron todos los destinatarios. Se aplican el plan, las cuotas y la disponibilidad regional del proveedor. **Sin verificar:** cuenta real, permisos efectivos, vencimiento/revocación de la clave, credenciales regionales y recorrido autorizado completo. Primer alcance de lectura, sin paridad total con el servicio.

Docs impact: README, bilingual listing, skill and capability matrix document least-privilege bearer auth, explicit regional routing, privacy and verification limits. No native OAuth, Email channel or protocol change.
