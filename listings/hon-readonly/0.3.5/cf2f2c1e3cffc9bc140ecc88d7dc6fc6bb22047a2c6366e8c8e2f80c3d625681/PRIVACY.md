# hOn community extension privacy

Since 0.3.2 the optional managed monitor also keeps at most 120 numeric observation samples. `hon_status`
can expose query times, connection/authentication booleans, allowlisted numeric mode/phase/program/time/error/
delay/door values and detector flags to the chat and configured model. No credentials, appliance identities or
raw responses are recorded. Results exclude samples older than 24 hours; managed polling prunes old rows.
When disabled, stored rows remain until removal but expired samples are not returned. See MONITOR.md.

Desde 0.3.2 el monitor opcional también guarda hasta 120 muestras numéricas. `hon_status` puede mostrar en
el chat y al modelo elegido horas de consulta, conexión/autenticación, valores permitidos de modo/fase/
programa/tiempo/error/demora/puerta y banderas del detector. No guarda claves, identidades ni respuestas
crudas. No muestra muestras de más de 24 horas; los sondeos borran filas antiguas. Si se desactiva, las filas
permanecen hasta quitar la extensión, pero no se devuelven las vencidas. Ver MONITOR.md.

Version scope: managed events and explicit starts below describe 0.3 and require an event-capable APX build. The stable 0.2.1 preview has no controls; its optional standalone observer uses an owner-chosen folder and a separately configured Flow, not managed delivery. That folder/Flow must be removed or disabled separately.

Alcance de versión: los eventos administrados y arranques descriptos abajo corresponden a 0.3 y requieren un APX compatible. El piloto estable 0.2.1 no tiene controles; usa una carpeta elegida y un Flow separado para observación opcional. Esa carpeta y ese Flow se quitan o desactivan aparte.

This is an unofficial wrapper's notice, not hOn's privacy policy or service terms. Review the hOn app's current account notices before connecting. APX's general privacy reference and this wrapper's MIT license do not replace the manufacturer's terms.

APX injects this extension's email/password secrets at runtime. pyhOn sends them to upstream authentication endpoints. Session tokens remain in the Python process, not wrapper state files. Diagnostic logging is disabled because upstream authentication errors can contain private data. Error messages are fixed categories.

Appliance listing, attributes and, for explicitly requested starts, command descriptors go to the hOn cloud. Status output omits names, free-text models, MACs, serials, raw responses and unknown fields. Bounded numeric household telemetry, query time and temporary appliance positions can enter APX chat history and the configured model provider. There is no additional analytics recipient. Query time is not proof of physically fresh device data.

The separately enabled monitor stores a cursor and a bounded queue of completion notices in APX's managed extension-data directory: random cycle ids, times, countdown/flags, configuration session and fixed completion messages, never credentials or appliance addresses. The core supplies the session; an ordinary tool call cannot reset it. Receipts are acknowledged after core persistence; terminal receipt deduplication in APX has a 24-hour retention window, not an unlimited exactly-once guarantee. Disabling notices cancels pending core delivery. Turning the extension off also revokes notice opt-in; reconnecting alone does not resume it. Removing it removes its managed data. No Flow or owner-selected directory is required. Historical independent preview Flows are not managed by this lifecycle and must be disabled separately.

Optional starts are off by default. The proposal database stores random request ids, allowlisted program choices, private hashes of the target and mandatory defaults, expiry and attempted/uncertain status. It stores no credentials or raw addresses. An owner-requested start transmits its program and parameters to the manufacturer's cloud; the normal APX approvals and owner-selected autonomy apply. A readiness argument is not authenticated proof of physical confirmation. The source prevents automatic retries of the same proposal after an uncertain attempt. These controls are separate from observation notices.

uv downloads Python/dependencies into its own cache; download hosts receive ordinary network metadata. This wrapper does not control upstream retention, account handling or security. Removing it does not delete the hOn account or existing APX chat history. See MONITOR.md and CONTROLS.md for bounds, failure recovery, the completed Automatic trial on the supported model and remaining physical-validation limits.

## Español

Este aviso corresponde al wrapper comunitario no oficial, no a la política ni a las condiciones de hOn. Revisá las de la app antes de conectar. APX inyecta las claves propias al ejecutar; pyhOn las envía a autenticación. Los tokens quedan en memoria, no en el estado local. Los diagnósticos están apagados y los errores usan categorías fijas.

Las consultas de aparatos, atributos y, para arranques explícitos, catálogo van a hOn. El resultado omite nombres, modelos libres, MAC, seriales y respuestas crudas. La telemetría numérica acotada puede describir actividad del hogar, quedar en el chat y llegar al proveedor de IA elegido. No agrega analítica ni demuestra frescura física.

El monitor opcional guarda cursor y cola acotada de avisos en la carpeta de datos administrada por APX: identificadores aleatorios, horarios, cuenta regresiva, sesión y mensajes fijos; no claves ni direcciones del aparato. APX fija la sesión y reconoce recibos persistidos. La deduplicación de recibos terminales dura 24 horas, no es una garantía ilimitada. Desactivar cancela pendientes; apagar la extensión revoca los avisos y reconectar no los reactiva. Quitarla borra sus datos administrados. No requiere un Flow ni elegir una carpeta. Los Flows independientes del piloto histórico se desactivan aparte.

Los arranques vienen apagados. Guardan propuestas aleatorias, programas permitidos, hashes privados, vencimiento y resultado incierto; nunca direcciones ni claves crudas. Un arranque pedido explícitamente transmite programa y parámetros a la nube y sigue las aprobaciones y autonomía normales de APX. Un argumento no prueba confirmación física tuya. No reenvía automáticamente una misma propuesta incierta; un aviso no la autoriza.

uv descarga Python y dependencias en su caché y sus servidores reciben metadatos de red. Quitar la extensión no borra la cuenta hOn ni el historial de APX. MONITOR.md y CONTROLS.md explican límites, la prueba completada de Automático en el modelo compatible y las validaciones físicas restantes.
