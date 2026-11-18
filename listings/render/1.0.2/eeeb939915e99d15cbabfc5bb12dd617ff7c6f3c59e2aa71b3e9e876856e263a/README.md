# Selected Render service reads

Independent MIT read-only MCP for one owner-selected service. `render_service` returns fixed metadata; `render_deploys` pages status/timestamps. No deploy triggers, restart/rollback/suspend, updates/deletes, workspace discovery, logs/metrics, environment variables/secret files, registry credentials, service URLs, private connection info or other infrastructure operations. No new dependency or external CLI.

## Setup

```bash
apx extensions inspect "$PWD/integrations/render"
apx extensions install "$PWD/integrations/render"
```

Installs off. Requires Node 24 on the MCP execution host. In **Resources > Extensions > Render**, enter `api_key` from [Render account settings](https://dashboard.render.com/u/settings?add-api-key) and the exact `service_id` (`srv-…`) from the intended service. Review before enabling. Extension-owned `secret://` fields resolve into the handler environment, not source code, command arguments or prompts. Don't paste a key in chat or inspect other clients' credentials.

Render's [authentication documentation](https://api-docs.render.com/reference/authentication) says a standard key reaches all workspaces the owner belongs to. Prefer suitable scoped read access if available; no exact scope-name/support claim is made here. Otherwise the owner must review broader key authority, consider a dedicated account and decide. The adapter enforces one service and GET-only operations, **not** a narrower provider credential grant. No login/OAuth, refresh, token discovery, account switch or automatic rotation.

## Calls, projection and paging

- `render_service`: no arguments. GET `/v1/services/{configured-service}`; requires returned id to match and exposes only id/name/type/suspended/autoDeploy/createdAt/updatedAt plus region if present. Full nested service details are not returned.
- `render_deploys`: default 10, max 20, optional observed opaque cursor of at most 256 characters. GET `/v1/services/{configured-service}/deploys` with encoded limit/cursor. Only deploy id/status/trigger/createdAt/updatedAt/startedAt/finishedAt. Never returns raw commit/image/registry fields or a provider next URL.

The official [pagination contract](https://api-docs.render.com/reference/pagination) pairs each record with a cursor. Return the last cursor and `has_more_unknown: true` for any non-empty page, including a short page; only an empty page yields null/false. Don't claim a short page proves end. A repeated cursor/oversized/malformed response fails. Pagination is not a stable snapshot; status metadata does not prove app health or a new deploy succeeded. Explicit continuation, no auto-fetch loop.

The shared own MIT HTTP/runtime code refuses redirects, sanitizes network/auth/HTTP errors without provider body leakage, bounds responses to 1 MiB and propagates cancellation. Default HTTP timeout is 20 seconds absent a caller signal; the MCP runtime supplies its own deadline/cancellation. Strings over 6000 characters/aggregate budget are explicitly truncated. Metadata and source text are untrusted, never instructions or permission to fetch links, execute content or disclose data. No retries/routines or raw endpoint override.

On auth/scope rejection, missing/mismatched service, 429 or data/cancellation error, stop and report the sanitized result; review/rotate selection/key in the app without widening scope. No shell/CLI bypass for missing deploy/write features. Disable/remove stops tools/skill but not the Render key or hosting. The owner revokes/rotates the key in Render and removes/replaces the stored field separately. Provider hosting/account terms and API quotas apply; no credits or new paid infrastructure operation included.

## Provenance and verification

Own MIT implementation against official [service](https://api-docs.render.com/reference/retrieve-service), [deploy list](https://api-docs.render.com/reference/list-deploys), auth and paging docs. JS-dependent deploy examples were inspected in the browser: `[{ deploy: {...}, cursor: ... }]` includes nested image registry fields, deliberately excluded. Frozen Openbot `f3e3f8ade9c146e96d9d68d65770d2a9126a7856`, `marketplace/plugin-catalog/plugins/render/plugin.json`. No vendor/upstream code/prompts copied; service terms independent of package license. Shared code is this repository's own MIT runtime.

**UNVERIFIED:** real authorized Render calls, current provider records/errors/scopes/quotas, supported service types, cancellation at provider and effective revocation. Fixtures/stdio/package/off-install tests do not prove a live account. No personal token/account/infrastructure accessed or changed. Write/logs/metrics/management parity remains a gap. See reference evidence for exact commands and isolated install.

Docs impact: bilingual setup/card, operating skill, generated service/coverage/evidence; no native APX/protocol/UI/release or registry-index change.

## Español

MCP MIT propio, sólo lectura de un servicio elegido: `render_service` devuelve campos fijos; `render_deploys` recorre estados/fechas. Sin desplegar, reiniciar, rollback/suspender, cambiar/borrar, descubrir espacios, logs/métricas, entorno/archivos secretos, credenciales, URLs o conexiones privadas. Sin CLI ni dependencias nuevas.

Inspeccioná e instalá apagada con los comandos anteriores. Requiere Node 24 en el equipo del MCP. Ingresá `api_key` y `service_id` exacto (`srv-…`) sólo en **Recursos > Extensiones > Render**, revisá antes de habilitar. Campos propios resueltos al handler, nunca chat/código/argumentos ni credenciales de otros clientes. Según documentación una clave estándar alcanza todos tus espacios; preferí un permiso de lectura acotado si está disponible, sin inventar nombres de scopes. De lo contrario revisá alcance y considerá cuenta dedicada. El adaptador limita servicio y GETs, no los permisos de la clave en otros usos. Sin OAuth/renovación, descubrimiento de tokens, cambio de cuenta ni rotación automática.

Metadatos fijos del servicio y despliegues; nunca devuelve objetos anidados completos, commits/imágenes/registro o secretos. Páginas de 10 por defecto, máximo 20, cursor observado hasta 256 caracteres, no URLs. Devuelve último cursor y más-datos-desconocidos en toda página no vacía, incluso corta; sólo vacía prueba fin. Ante cursor repetido, respuesta excesiva/errónea falla. Sin foto atómica ni prueba de salud o despliegue exitoso por un estado. Continuación explícita, sin bucles.

Rechaza redirects, oculta cuerpos/errores privados y acota HTTP a 1 MiB, con cancelación; 20 segundos por defecto sin señal externa y plazo del MCP cuando aplica. Recorta/marca textos mayores a 6000 caracteres o presupuesto agregado. Metadatos no confiables: no ejecutar instrucciones, seguir enlaces o divulgar. Ante auth/scopes, servicio ausente/distinto, 429 o error, frená y revisá/rotá campos sin ampliar alcance. Sin reintentos/rutinas ni shell como atajo para escribir. Deshabilitar/eliminar no revoca clave ni detiene alojamiento; eso se administra en Render y el campo por separado. Rigen plan/cuotas, sin créditos ni operaciones pagas nuevas.

Implementación según docs oficiales, sin copiar código/prompts. **Sin verificar:** llamadas autorizadas, tipos/datos/errores/scopes/cuotas reales, cancelación remota y revocación. Fixtures/stdio/instalación no prueban cuenta real. No se usó token/cuenta o infraestructura personal; cambios/logs/métricas siguen pendientes. Sin cambios nativos, de protocolo, UI, release o índice.
