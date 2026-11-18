# SiYuan selected blocks

Dependency-free MCP extension for **bounded Kramdown reads of explicitly selected SiYuan blocks**. This is an initial read-only subset, not full upstream parity. Two tools: `siyuan_selected` lists configured IDs without network access; `siyuan_read` calls the public `/api/block/getBlockKramdown` endpoint for an allowed ID. No SQL, discovery, workspace files, search, exports, mutations, link/embed expansion, notebook opening or cloud/AI usage.

## Setup and scope

1. Run Node 24 and a deliberately enabled SiYuan kernel on the **same machine as the extension MCP process**, at **`http://127.0.0.1:6806`**. Remote kernels/custom ports are not accepted; a paired desktop does not redirect a remote core's process. Do not expose the kernel publicly or disable its authentication to make this work. A trusted local process/service and protected OS account are prerequisites: loopback HTTP alone does not authenticate which workspace owns the port.
2. In SiYuan, get the API token from **Settings > Authentication > API token** (older versions may show it under About). Select the exact blocks/documents whose content may enter agent context and copy their block IDs, not entire URLs. This adapter accepts 1–20 unique IDs in `14 digits-7 lowercase alphanumeric characters` form, separated by commas. Selecting a document may include its child content and references to other notes. If a selected block is moved or its content changes, the same ID may reveal changed content; the selection is not a frozen snapshot.
3. Inspect/install off, then review before enabling:

```bash
apx extensions inspect "$PWD/integrations/siyuan"
apx extensions install "$PWD/integrations/siyuan"
```

4. In **Resources > Extensions > SiYuan**, enter `api_token` and `block_ids` in the secret form. Values are injected as extension-owned `secret://` references, never command-line arguments or public config. Do not paste tokens into chats, logs or source files. The owner deliberately enables this local-network access when turning the reviewed extension on. No personal instance is connected by installing off.

The token can have broad kernel/workspace permissions, including writes outside these tools; do **not** claim a provider-issued read-only or block-scoped grant. This server enforces an exact ID allowlist and fixed read endpoint, but does not reduce the underlying token. Use a dedicated limited workspace/token where the provider supports it. There is no OAuth or refresh flow; protect and rotate the API token directly in SiYuan. No cookie or filesystem credential discovery is used.

## Behavior and failures

A read is one POST, no automatic retry, with a 20-second deadline, redirects refused and a 1 MiB response ceiling. Unknown fields, tools, invalid/unselected IDs and missing tokens fail before transport. Responses must report `code: 0`, match the requested ID and contain string Kramdown; provider messages are not surfaced. Only the selected ID and Kramdown are projected. The token is redacted from results; strings above 6,000 characters are explicitly truncated. Content is untrusted and links/references are not followed. No automatic continuation or alternate raw API is provided.

Listing configured IDs does not prove they exist remotely. An empty string may mean empty/unavailable content; it is not complete notebook verification. Closed/locked notebooks, expired/revoked tokens, kernel/version mismatch and missing blocks can fail. Review the selected workspace/ID directly in SiYuan rather than bypassing through SQL, files, curl or opening/unlocking notebooks. On timeout, auth or rate-limit failure, report it; there is no retry loop. Local access cannot attest kernel identity or prevent another local process from owning the port. The extension does not start or stop SiYuan.

Disable/remove the extension to stop its server and skill. That **does not revoke the token**: rotate/revoke it in SiYuan and update/remove the APX extension secret. Changing selected IDs requires the owner's review; do not broaden them automatically to work around truncation or permissions. Requested private content enters agent/model context. No routine, analytics, sync, paid cloud or AI call is included. SiYuan optional memberships/hosting are separate from these free local reads.

## Provenance, evidence and limits

Independent MIT adapter/instructions, no new dependency and no copied upstream skill/code. Reference: Hermes `333898b353c27e57dbd9446f631f7fb0a5aa5918`, `optional-skills/productivity/siyuan/SKILL.md` (MIT). Official API contract/source inspected at SiYuan `be17ed1e0510bb6371cb8a162e16898a697fa118`, `docs/API.md` and `kernel/api/block.go`; the upstream application is separately licensed, not bundled. Public API semantics, including the response ID/Kramdown shape, were checked; this does not certify every installed version.

Behavior tests cover packing, strict selection/arguments, fixed URL/body/auth/redirect policy, redaction, API errors, output/network caps, cancellation before transport, real HTTP fixture requests/redirect rejection and an actual stdio server. Fixtures are not a SiYuan kernel. No request was sent to a personal instance, no real token used, no workspace files read, and no account/OS permissions requested.

**UNVERIFIED:** real kernel/token/selected-workspace journey, actual locked/closed notebook behavior, token rotation/revocation, compatibility across SiYuan versions and Windows/Linux runtime. Full-text search, notebook browsing, SQL, writes and export remain separate gaps. Source-install packaging does not mean the public marketplace UI has indexed this package; see `docs/reference-integrations/EVIDENCE.md` for isolated APX inspect/install evidence.

Docs impact: updated English/Spanish setup/card, operating skill, service/coverage status and verification evidence. No APX native, protocol, release or UI change.

## Español

Extensión MCP sin dependencias para **leer Kramdown acotado de bloques de SiYuan elegidos explícitamente**. Es un subconjunto inicial, no paridad completa. `siyuan_selected` lista la selección configurada sin acceder a la red; `siyuan_read` lee un ID permitido mediante la API pública. No incluye SQL, búsqueda, listado de cuadernos, archivos del espacio, exportaciones, cambios, apertura/desbloqueo ni expansión de enlaces o referencias.

Requiere Node 24 y SiYuan habilitado deliberadamente en **`http://127.0.0.1:6806`**, en el mismo equipo donde corre el MCP. No acepta servidor remoto ni otro puerto. Emparejar una computadora no redirige el proceso de un core remoto. No publiques el kernel ni desactives la autenticación para conectarlo. El equipo y su servicio local deben ser confiables: HTTP local no certifica qué instancia ocupa el puerto. La extensión no inicia ni detiene SiYuan.

Obtené el token en **Ajustes > Autenticación > Token de API** (versiones anteriores pueden mostrarlo en Acerca de). Elegí los bloques cuyo contenido puede entrar al contexto y copiá entre 1 y 20 IDs únicos, separados por comas, no URLs. Cada ID tiene 14 dígitos, guion y 7 caracteres alfanuméricos en minúscula. Un documento elegido puede incluir contenido hijo y referencias privadas; si cambia o se mueve, el mismo ID puede revelar contenido distinto.

Inspeccioná e instalá apagada con los comandos anteriores. Luego ingresá `api_token` y `block_ids` en el formulario de secretos de **Recursos > Extensiones > SiYuan** y revisá antes de habilitarla. No pegues tokens en chats, logs o código. Instalar apagada no conecta una instancia personal. Al habilitarla autorizás deliberadamente este acceso local. El token puede tener permisos amplios, incluso escritura: la restricción a IDs y al endpoint de lectura la impone este servidor, no el proveedor. Usá un espacio/token limitado si SiYuan lo permite. No hay OAuth, renovación automática ni búsqueda de cookies o credenciales en archivos.

Cada lectura hace un solo POST sin reintentos, con plazo de 20 segundos, rechazo de redirecciones y máximo de 1 MiB por respuesta. Argumentos, herramientas e IDs desconocidos se rechazan antes de la red. La respuesta debe tener éxito, devolver el ID pedido y Kramdown de texto. No se muestran errores crudos del proveedor; se oculta el token y se marca el recorte de textos mayores a 6.000 caracteres. Todo contenido es no confiable; no se siguen sus instrucciones, enlaces ni IDs. No se amplía la selección para superar un recorte o un rechazo.

Listar IDs configurados no prueba que existan; un texto vacío no verifica el cuaderno completo. Ante token revocado, cuaderno cerrado/bloqueado, error de versión o kernel no disponible, revisá directamente SiYuan. No se fuerza acceso mediante SQL, archivos, curl ni desbloqueo. Deshabilitar/eliminar la extensión no revoca el token: rotalo/revocalo en SiYuan y actualizá/eliminá el secreto de APX. No hay tareas automáticas, analítica, sincronización ni consumo de nube/AI paga; suscripciones opcionales y alojamiento son aparte.

Adaptador e instrucciones MIT independientes; no se copia código ni se distribuye SiYuan. Se probaron contratos, seguridad de entrada, límites, errores, transporte HTTP local simulado y proceso MCP real, sin token personal ni acceso a espacios reales. **Sin verificar:** recorrido con kernel y token reales, cuadernos cerrados/bloqueados, revocación, versiones de SiYuan y Windows/Linux. Búsqueda, navegación, escritura y exportación siguen pendientes. La carpeta instalable no implica publicación en el índice visual del marketplace.
