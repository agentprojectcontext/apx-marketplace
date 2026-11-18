# Resend status reads

Independent MIT Node 24 MCP: two fixed GET-only operations, no new dependency/CLI. `resend_domain` reads one configured domain's selected status/region/tracking and DNS-record status metadata, **not DNS values**. `resend_email_status` reads id/created_at/last_event/scheduled_at for one owner-provided or previously observed sent-email id. No send/resend, discovery, verification trigger, updates/deletion/cancellation, inbox, contacts/broadcasts/webhooks or credential APIs. This is provider administration/delivery metadata, not a duplicate of native Gmail/mail and not full upstream parity.

## Setup

```bash
apx extensions inspect "$PWD/integrations/resend"
apx extensions install "$PWD/integrations/resend"
```

Installs off. In **Resources > Extensions > Resend**, enter `api_key` from [Resend API keys](https://resend.com/api-keys) and exact `domain_id`. Review before enabling; requires Node 24 on execution host. Extension-owned `secret://` references resolve inside the handler environment, not code/arguments/prompts. No OAuth/refresh, key creation/discovery, external credential cache or credential lookup. Never paste a key in chat.

[Documented key permissions](https://resend.com/docs/api-reference/api-keys/create-api-key) include full access/sending access and optional domain restriction; this package does not invent read-only scopes. Use the minimum grant that actually permits chosen reads and a domain restriction where supported; a sending-only key may not work. Broader access needs owner review, not automatic widening. The adapter's no-write boundary does not reduce credential authority elsewhere. **Configured domain applies only to the domain tool, not the email tool**. Email IDs must be observed/provided; no guessing/discovery or claim of domain isolation for those reads.

## Projection, failure and removal

Fixed `https://api.resend.com/domains/{configured-uuid}` and `/emails/{observed-uuid}` GETs; strict UUID/input allowlists, returned id match, fixed projection/field-type checks. No redirects, endpoint/method/header override, pagination, hidden calls, retries or routines. API email response contains body/subject/addresses processed transiently inside the handler, but these plus tags/message id are not returned to agent context. Domain DNS values/extra fields are also dropped. Only selected metadata enters context; it remains untrusted, never instructions or permission to execute/fetch/disclose. Metadata/last_event does not prove human reading, inbox placement or live health.

Shared own MIT runtime/HTTP bounds source to 1 MiB, sanitizes provider/network/auth errors without bodies, redacts the configured key, marks strings beyond 6000 characters/aggregate context truncation and propagates deadline/cancellation. Default HTTP timeout 20 seconds absent external signal; MCP supplies its own signal. Malformed/mismatched resource, bad fields or >64 DNS rows fail closed. On 401/403/404/429/errors stop and review resource/key directly, no account switching or CLI bypass. Provider plan/quota terms apply; no credits or sending/new paid operation included.

Disable/remove stops tools/skill, not key revocation or already scheduled emails. Owner revokes key in Resend and removes/rotates extension field separately. Broader sending/domain-management upstream parity remains a gap, not implicitly supported by a stored full-access credential.

## Provenance and verification

Original MIT code/skill based on official [domain read](https://resend.com/docs/api-reference/domains/get-domain), [sent email read](https://resend.com/docs/api-reference/emails/retrieve-email), [key management](https://resend.com/docs/dashboard/api-keys/introduction) and key-permission docs. Frozen Openbot `f3e3f8ade9c146e96d9d68d65770d2a9126a7856`, `marketplace/plugin-catalog/plugins/resend/plugin.json`; no vendor/upstream code/prompts copied. Own existing MIT HTTP/runtime reused; provider terms independent of package license.

**UNVERIFIED:** actual authorized provider calls/payloads, effective permissions/domain restriction, current errors/quotas/cancellation and key revocation. Behavioral fixtures, actual stdio and archive/off-install do not prove a provider account. No owner key/account/email was accessed or sent. Exact tests/install evidence is recorded in reference evidence when run.

Docs impact: bilingual card/setup, operating skill, generated service/coverage and evidence. No native APX/protocol/UI/release/registry-index change.

## Español

MCP MIT propio, Node 24, dos GETs fijos: `resend_domain` lee estado/región/tracking y estado DNS de un dominio, sin valores; `resend_email_status` lee ID/fechas/último evento de un email enviado por ID provisto/observado. Sin enviar/reenviar, descubrir, verificar, cambiar/borrar/cancelar, inbox, contactos/broadcasts/webhooks o credenciales. Es estado administrativo/entrega del proveedor, no Gmail/mail ni paridad completa.

Instalá apagada con los comandos anteriores. Ingresá `api_key` y `domain_id` exacto sólo en **Recursos > Extensiones > Resend**, revisá y habilitá. Campos propios resueltos al handler; nunca chat/código/argumentos ni credenciales ajenas. Sin OAuth/renovación, creación/descubrimiento de claves o caché externa. Permisos documentados full access/sending access y restricción opcional de dominio, sin inventar scope de lectura: usá mínimo permiso que funcione y restricción cuando corresponda. Clave sólo de envío puede no permitir leer; acceso más amplio lo decide el dueño, nunca ampliación automática. Límite sin escrituras del adaptador no reduce permisos de la clave en otros usos. **Dominio sólo limita su herramienta, no emails**: IDs observados/provistos, sin adivinar ni afirmar aislamiento por dominio.

GETs a origen fijo, UUIDs y campos validados con identidad de respuesta; sin URLs/métodos/cabeceras externos, paginación, llamadas ocultas, reintentos/rutinas. API devuelve cuerpo/asunto/direcciones transitoriamente al handler; no llegan al contexto, tampoco tags/message-id/valores DNS. Metadatos no confiables, nunca ejecutar instrucciones, seguir enlaces o divulgar. Último evento no prueba lectura humana, inbox o salud fresca.

Acota HTTP a 1 MiB, errores sin cuerpos privados, redacta clave, marca recorte a 6000 caracteres/presupuesto agregado y cancela/plazo MCP; 20 segundos por defecto sin señal externa. Campos/ID erróneos o más de 64 filas DNS fallan. Ante auth, recurso ausente, 429/error frená y revisá/rotá campos sin cambiar cuenta, ampliar o usar CLI. Rigen plan/cuotas; sin créditos/envíos/operaciones pagas nuevas. Deshabilitar/eliminar no revoca clave ni emails programados: se administran en Resend y el campo por separado.

Docs oficiales y código/skill originales; términos del proveedor aparte. **Sin verificar:** cuenta/llamadas/datos/permisos/restricción de dominio, errores/cuotas/cancelación y revocación reales. Fixtures/stdio/archivo/instalación no prueban cuenta. No se usó clave/cuenta/email personal. Cambios/envíos y gestión más amplia pendientes; sin cambios nativos/UI/release/índice.
