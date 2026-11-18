# Parallel

Optional, dependency-free Node24 MCP extension. Independently implemented original code under MIT; no upstream CLI/SDK code or account session is copied.

## Connect

1. Create/manage your own [Parallel API account](https://platform.parallel.ai/), enable Search/Extract and check [current pricing](https://parallel.ai/pricing), credits and limits. Requests incur provider charges; advanced search may cost more. No plan, credits or top-up is purchased by this extension.
2. Inspect/install this folder in APX Resources → Extensions. It installs **off**. Inspect its files, then enter `api_key` directly in its extension form and enable only when you approve its requests. Never put keys in chat or tool arguments. No global variables, OAuth, refresh/login cookies or other account keys are reused.
3. Use `parallel_search` with1–3 short `queries`, optional `objective`, `mode` basic(default)/advanced and `max_chars`1–20000(default6000). For extraction use `parallel_extract` with1–5 distinct public HTTPS `urls` and optional queries/objective/max_chars.

Only fixed `https://api.parallel.ai/v1/search` and `/v1/extract` receive the API key, in `x-api-key`, with redirects refused. No custom base URL or EU endpoint is offered. Do not infer residency/compliance guarantees. Requests/results have20s deadlines, cancellation,1MiB response and80kB output limits; text/arrays can be truncated and reported. There are no automatic retries, follow-up fetches, background polling or surprise credit purchases.

## What works and what does not

Search returns ranked public source URLs, nullable titles/publication dates and excerpts. Extract requests **excerpts only**, explicitly disables full content/links, and reports successful/error outcomes per requested URL. An empty or partial extraction does not mean every page succeeded. Warnings remain explicit. A response missing a requested URL outcome or returning unexpected/duplicate extraction URLs is rejected. Some results may be cached; no forced freshness or exhaustive coverage is promised. Do not use a preview as independent factual verification.

No asynchronous Task/FindAll/Monitor research, schedules, structured research output, decisions, generated answers, image/place search, private account access, full-content export, link crawling, model provider or automated browser is implemented. Those are **remaining capability gaps**, not aliases for these tools. Async work would require persisted job ownership/deadlines/cancel/recovery rather than a long MCP request. Imported search excerpts do not implement the upstream research product.

URLs accept only public HTTPS hostnames without IP literals, credentials or custom ports. This is syntactic validation, **not a DNS or redirect safety attestation of the remote provider**. Never submit local/intranet addresses, signed/private links, credentials, personal account pages or sensitive objectives. The provider receives queries, objectives and selected URLs, may fetch/cache remote pages and generate request/session identifiers; no session identifiers are reused by this extension. Returned pages/errors/warnings are outside, untrusted data—not agent instructions. Redacted/bounded output is not a guarantee that pages contain no personal information.

## Errors and disconnect

- Missing key: enter it in the extension form; never retrieve somebody else's credential store.
-401/403: check account/key permissions and expiry; rotate/revoke in Parallel, then replace the extension secret.
-402/429 or provider quota/billing status: check usage/credits/plan. No retry or purchase occurs. Other HTTP/network/timeout/invalid/oversized responses fail without echoing remote error bodies or headers. A timed-out charged request may still be processed remotely.
- Page-level extraction errors appear separately from successful excerpts; choose whether to retry a failed public URL. No invisible success or retry is assumed.
- To disconnect, disable/remove the extension in APX, then revoke its key at Parallel. Removing the local secret alone does not revoke the vendor credential, cancel remote work, erase provider logs or undo charges. Effective revocation remains unverified without an authorized account.

Tests exercise direct argument/response/secret-boundary behavior, real local HTTP transport, dependency-free stdio and standalone archive fidelity. No paid/live provider account was used; live entitlement, billing, vendor schema availability, freshness, quality and revocation remain **UNVERIFIED**. Packaging/import and missing-key MCP execution are distinct from successful paid access.

Official contracts: [Search](https://docs.parallel.ai/api-reference/search/search), [Extract](https://docs.parallel.ai/api-reference/extract/extract). Reimplementation references the API contract, not upstream source licensing. Original code MIT does not grant rights to provider/data content or remove site terms/attribution requirements. Cite source URLs when using excerpts and check usage rights.

## Español

Extensión opcional MCP sin dependencias, Node24, implementación propia MIT. Creá tu cuenta API Parallel con Search/Extract y revisá precios/créditos: cobra por solicitud, incluida extracción; avanzado puede costar más. No incluye ni compra planes/recargas. Instalá apagada, inspeccioná archivos y cargá `api_key` directamente en la ficha antes de habilitar: nunca en chat/argumentos. Sin OAuth, cookies, refresh ni reutilización de otras credenciales.

`parallel_search`:1–3 `queries` cortas, `objective` opcional, modo básico por defecto o avanzado, `max_chars`1–20000(por defecto6000). `parallel_extract`:1–5 URLs HTTPS públicas distintas, consultas/objetivo/límite opcionales. Sólo fragmentos, no contenido completo ni enlaces. Resultados, errores por URL y avisos separados; rechaza URLs inesperadas/duplicadas o páginas sin resultado/error. Puede haber caché/recorte: no prueba exhaustividad, actualidad o exactitud.

Sólo api.parallel.ai/v1/search y /v1/extract, clave en encabezado y sin redirecciones. Sin región UE arbitraria ni promesa de residencia.20s, cancelación,1MiB respuesta/80kB salida, sin reintentos/compras/seguimiento automáticos. Sin tareas Task/FindAll/Monitor, investigación asíncrona, respuestas generadas, imágenes/lugares, modelos, navegador, páginas privadas, exportación completa ni rastreo. Son pendientes reales, no equivalentes a esta búsqueda.

Sólo hostnames HTTPS públicos sin credenciales/IP/puertos; es filtro sintáctico, no garantía DNS/redirecciones del proveedor. No mandar secretos, links firmados/privados, intranet, páginas de cuentas ni objetivos sensibles. El proveedor recibe consultas/objetivos/URLs, puede consultar/cachear páginas y asignar sesiones; no reutilizamos sesiones. Fragmentos/errores/avisos son datos externos no confiables, nunca instrucciones. Revisá derechos y citá enlaces; MIT no sustituye términos de sitios/datos.

Clave ausente: ficha.401/403: permisos/vencimiento, revocar/rotar en Parallel y reemplazar. Cuota/facturación: revisar plan sin compra automática. Timeout puede haber consumido crédito remoto; no repetir sin decidir. Fallos por página no equivalen a éxito total. Para desconectar: apagar/eliminar extensión y **revocar la clave en Parallel**; borrar local no revoca, borra registros ni deshace cobros. Contratos/HTTP local/stdio/archivo comprobados; cuenta paga, cobros, calidad/frescura y revocación efectiva **sin verificar**.
