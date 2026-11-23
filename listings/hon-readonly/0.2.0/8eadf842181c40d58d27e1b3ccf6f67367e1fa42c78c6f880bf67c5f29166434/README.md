# hOn read-only (community)

An unofficial community connection to the hOn cloud, using [pyhOn 0.17.5](https://github.com/Andre0512/pyhOn). Not made, endorsed or supported by Haier, Candy, Hoover or hOn. Requires internet and the hOn cloud; undocumented cloud APIs may change without notice.

## Connect

1. Install [uv](https://docs.astral.sh/uv/getting-started/installation/) on the computer running APX, and make `uv` available on APX's PATH. Restart APX yourself if you changed its PATH. No local virtual environment or Node packages are bundled.
2. Install this extension from Marketplace. It installs off. Open its page and enter **hOn email** and **hOn password** in its private credential fields, then turn it on when ready. These are extension-owned secrets (`hon_email`, `hon_password`), not shared variables or another connector's credentials. Never paste credentials into chat.
3. Use an email/password login accepted by the hOn app. Interactive Google sign-in is not supported. If your account uses Google, establish a hOn password for that same account through hOn's supported account recovery/password flow and verify it in hOn first. This extension cannot set or reset it.
4. Ask to check hOn appliance status with `hon_status`, with no arguments and no appliance controls. The optional `hon_monitor_poll` observation tool writes local state only when configured and enabled; it is not a device control. See [MONITOR.md](MONITOR.md) for its limited prewash mapping, separate Flow setup, at-most-once delivery and unverified end-to-end preview status.

At first launch uv may download Python 3.13 and the dependencies in `server.py.lock` into its own cache. The PEP 723 script pins pyhOn 0.17.5, MCP Python 1.30.0 and aiohttp 3.14.5; the lock pins all transitive packages and artifact hashes. `--locked` refuses a stale/missing lock rather than silently updating it. Downloads require internet, disk space and access to Python distribution/PyPI hosts. On other operating systems the same lock selects platform-specific wheels; only macOS ARM64 has been exercised for this package. A missing uv executable or an unavailable wheel prevents startup before the tool can answer. No installer runs during APX extension inspection.

## What it reads

Authentication → appliance list → attributes for up to 50 appliances → session close. The status tool uses no MQTT, command submission, program start/stop/pause, appliance settings changes, background polling or file persistence. The separately enabled monitor stores local observation state; it needs an explicitly configured Flow for scheduled polls and delivery. Login necessarily sends credentials to the upstream authentication service and obtains session tokens in memory.

The response includes the query time, temporary appliance numbers, a known type code, a known connection category, allowlisted numeric telemetry and a fixed monitor-profile marker only for the exact supported model. It deliberately excludes free-text names/models, MACs, serial numbers, tokens, unknown parameter names and nested objects. Numbers identify positions in this response only. Several appliances of the same type cannot be reliably distinguished here. Numeric codes are model-specific: there is no verified generic mapping of running/finished, program names, errors or time units. Query time is not a device freshness guarantee.

Only **Candy CF 6C4F1PS-12 (DW dishwasher)** has physical pilot read evidence. Other models/brands that pyhOn supports are not verified with this extension. Unknown or nonnumeric fields are omitted; `telemetry_available: false` means no supported values, not an idle/offline device. Cloud readings can be stale; use the appliance/hOn app for safety-relevant decisions.

## Troubleshooting

- `missing_credentials`: enter both credentials on the extension page; unresolved secret references are refused.
- `authentication_failed`: hOn did not complete login. Check the account/password in hOn, and cloud availability. This intentionally does not distinguish invalid credentials from authentication service failure.
- `cloud_unavailable`: listing/status failed after login, or the response was unexpected. Check hOn and internet access; retry later.
- `timeout`: a read exceeded 45 seconds (API cleanup and HTTP session cleanup each have a separate 5-second limit). No partial status is returned on failure. APX may impose its own earlier tool deadline.
- An authenticated empty `appliances` list means no usable appliances were returned for that account. Check account pairing in hOn. Malformed entries are counted as skipped; more than 50 entries are truncated explicitly.
- Unknown fields do not get guessed or decoded. Turn the extension off to stop its MCP process; remove it to uninstall. This does not modify your hOn account or appliances.

## Privacy, license and maintenance

Credentials are injected at runtime by APX and never included in the package. Upstream logging is suppressed because authentication diagnostics can contain sensitive response bodies. Tool errors use fixed categories, not exception text. Selected status results can enter chat history and the configured model provider. There is no extra analytics service. This is a privacy-reduced status reader, not a promise about the upstream cloud's data handling.

The wrapper is MIT licensed (LICENSE). pyhOn and MCP Python are installed from their locked distributions, not vendored; each retains its own license and notices. See THIRD-PARTY.md. They are needed to reuse hOn's community authentication/attribute API and the Python MCP protocol rather than reimplementing either. aiohttp was already a locked transitive dependency; it is declared directly because the wrapper now owns and closes the shared HTTP session, including upstream authentication recovery and API cleanup failures. This adds no new distribution. Native dependency wheels may be substantial at first use; runtime is a separate on-demand Python process. Review dependency updates and regenerate the lock before publishing a new version. Never rewrite a published archive.

## Español

Conexión comunitaria **no oficial** a la nube hOn, sin respaldo de Haier, Candy, Hoover ni hOn. Requiere internet; la API puede cambiar. Instalá uv en la computadora que ejecuta APX y asegurate de que esté disponible para APX. La extensión se instala apagada: cargá el email y la contraseña hOn en su ficha (no en el chat), y activala cuando estés listo. Usa secretos propios y no reutiliza los de otro conector. El inicio interactivo con Google no está soportado; primero definí una contraseña hOn para la misma cuenta por el mecanismo admitido por hOn y comprobala en la app.

Al iniciar, uv puede descargar Python 3.13 y las dependencias fijadas con hashes en server.py.lock. No incluye un entorno virtual local. Sólo se probó el runtime de este paquete en macOS ARM64; otras plataformas y sus paquetes quedan sin verificar. Si falta uv o no hay una distribución compatible, el servidor no inicia. Inspeccionar la extensión no instala dependencias.

La herramienta hon_status consulta la cuenta y los atributos de hasta 50 aparatos y cierra la sesión. No envía comandos, cambia ajustes, inicia/pausa/detiene programas, usa MQTT ni sondea en segundo plano. Sólo el lavavajillas **Candy CF 6C4F1PS-12 (DW)** tiene evidencia física del piloto. Otros modelos no están verificados. Devuelve la hora de consulta, posiciones temporales, tipo, conexión y telemetría numérica permitida. Omite nombres, modelos libres, MAC, seriales, tokens, campos desconocidos y objetos anidados. No identifica de forma estable varios equipos del mismo tipo ni interpreta códigos, programas, errores o unidades sin un mapeo validado. La hora de consulta no demuestra que el aparato haya actualizado los datos.

El monitor opcional hon_monitor_poll viene apagado y guarda estado local sólo si se configura y activa. No controla el aparato ni avisa solo: necesita un Flow explícito. Su reconocimiento de fin está limitado al Pre-lavado observado; la entrega es como máximo una vez y puede perderse ante fallos. La prueba completa de programación y entrega sigue pendiente; no activarlo para uso desatendido. Ver MONITOR.md en español para configuración, privacidad, deduplicación y límites de frescura.

missing_credentials pide completar las claves; authentication_failed puede ser contraseña inválida o falla de autenticación en la nube; cloud_unavailable indica error o respuesta inesperada tras el login; timeout limita la lectura a 45 segundos más hasta 5 para cerrar la API y otros 5 para cerrar la sesión HTTP, incluso si falla el cierre de la API. El wrapper es dueño de la sesión compartida también durante la recuperación de autenticación; aiohttp ya estaba fijado como dependencia transitiva y ahora se declara directamente, sin agregar otra distribución. Un fallo no devuelve estados parciales. Una lista vacía no significa que el aparato esté apagado: revisá la cuenta y el emparejamiento en hOn. Los equipos malformados se cuentan como omitidos y la truncación se informa. telemetry_available falso significa que faltan datos compatibles. Para decisiones de seguridad, consultá el aparato o la app.

Las credenciales sólo se inyectan al ejecutar; los errores no revelan diagnósticos privados. Los resultados elegidos pueden quedar en el chat y llegar al proveedor de IA. No agrega analítica. El wrapper es MIT; pyhOn y MCP conservan sus licencias en sus distribuciones, sin código vendorizado. Para desactivar, apagá la extensión; quitarla no modifica la cuenta ni los aparatos. Las actualizaciones requieren revisar dependencias y regenerar el lock en una versión nueva.

Docs impact: updated — this README and the bilingual listing document setup, runtime downloads, read-only/privacy boundaries, compatibility and failure recovery. No APX core behavior changes.
