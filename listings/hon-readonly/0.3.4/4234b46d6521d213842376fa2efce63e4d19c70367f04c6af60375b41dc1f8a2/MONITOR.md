# Managed hOn observations

## Missing notices and automatic door opening

From 0.3.3, diagnostic `emitted` is true only when a new event was inserted into the local outbox. It does
not prove core receipt or chat delivery. A full outbox reports `source_storage_full` with `emitted: false`;
a later successful retry records true. Desde 0.3.3, `emitted` solo indica una inserción nueva en la cola
local, no recepción en APX ni entrega al chat. Si la cola está llena informa `source_storage_full` y
`emitted: false`; un reintento posterior que logra insertarlo registra true.

Version 0.3.2 records up to 120 managed observations, retained for at most 24 hours in query results.
`hon_status` includes `observation` with these samples when managed storage is configured. Reading them does
not poll the monitor, advance detection, acknowledge events or control the appliance. Samples contain query
times, authentication/connection booleans, allowlisted numeric mode/phase/program/countdown/error/delay/door
values and detector flags. They contain no device identities, credentials or raw responses. Old evidence is
pruned on subsequent managed polls; with observation disabled, the file remains until extension removal but
expired samples are not returned. This is bounded troubleshooting evidence, not physically fresh telemetry.

Automatic door opening can leave the cloud in idle before the required two completed readings. That path
has not passed physical end-to-end validation. An open door, idle state or elapsed countdown alone is not
proof of successful completion and must not be relabeled as a completed cycle. Do not retry a program to
compensate for a missing notice. Diagnose the sampled transition first; no retrospective event is generated.

### Español

Desde 0.3.2 se guardan hasta 120 observaciones administradas; la consulta muestra solo las últimas 24 horas.
`hon_status` incluye `observation` si hay almacenamiento administrado. Leerlo no sondea el monitor, no avanza
el detector, no reconoce avisos ni controla el aparato. Guarda horas de consulta, indicadores de conexión y
autenticación, valores numéricos permitidos de modo/fase/programa/tiempo/error/demora/puerta y banderas del
detector; nunca identidades, claves ni respuestas crudas. Las siguientes observaciones eliminan muestras
viejas. Si se desactiva, el archivo permanece hasta quitar la extensión, pero no se devuelven muestras
vencidas. Es evidencia acotada de diagnóstico, no prueba de telemetría físicamente fresca.

La apertura automática puede dejar la nube en espera antes de las dos lecturas de fin requeridas. Esa ruta
no tiene validación física completa. Puerta abierta, espera o cuenta regresiva agotada no prueban un fin
exitoso y no se convierten en un aviso. No repitas un programa para compensar un aviso perdido: primero
revisá las muestras. No se genera un evento retrospectivo.

Version 0.3 declares `cycle_finished` and the `hon_events_poll` observation tool in its own `status` MCP server.
It requires APX extension events. No Flow, extra timer process, APX authentication token or model turn per poll
is required. Connecting alone does not opt in: separately choose notices, a personal chat and quiet hours in
this extension's settings, or ask your agent to configure them.

The interval is five minutes in idle and one minute during any recognized running program so short
cycles can provide countdown samples. Two spaced completed samples are required. A cycle wholly between idle
checks may be missed. Poll intervals are not deadlines or guarantees of physically fresh cloud telemetry.

The core reserves `hon_events_poll` for managed observations: ordinary agent discovery and MCP calls cannot
invoke it. It supplies `APX_EXTENSION_DATA_DIR` outside the fingerprinted package and pins the configuration
session through `APX_EXTENSION_MONITOR_SESSION`; a different caller-provided session is rejected before I/O.
The input is `{session, acknowledged: [{event,id}]}` with up to ten acknowledgments. This local bookkeeping is
explicitly declared in the manifest, non-destructive but not strictly read-only. Credentials and raw device
identifiers are never written to this state. A cloud error disarms the detector, never means completion.

A bounded SQLite outbox retains up to 1000 individual completions and replays the oldest ten until the core
acknowledges their persisted receipts. A second completion does not overwrite the first. Receipt persistence,
not an external message, is acknowledged: the core owns subsequent pending delivery. Lost acknowledgments and
restarts replay stable ids safely within the core's documented deduplication window. If the source fills up,
it keeps draining old events and reports backpressure instead of replacing them; a new completion may be missed
if its evidence ends before capacity recovers. This is bounded recovery, not unlimited lossless storage.

A changed configuration session intentionally resets arming and its old outbox. A core restart does not. Turning
notices off cancels pending delivery; turning the extension off also revokes opt-in, and reconnecting alone does
not resume it. Changed extension files need approval and configuration again. Core terminal receipts are retained
for 24 hours; pending receipts expire after seven days. At most 10,000 receipts and 60 new receipts per minute
are accepted per installation. Old terminal receipts are compacted automatically so capacity is recoverable.
A replay outside the deduplication window can become a new receipt; no permanent exactly-once claim is made.

Detection is model-scoped to the Candy CF 6C4F1PS-12 dishwasher, not program-scoped. All numeric program codes
(0–65535) use execution mode 2 with phase 1/2/3/4/6 and countdown progress, followed by end mode 7/phase 5 and
zero time twice, at least a minute apart. The same program code must persist through the observed cycle.
No program-name catalog is required: Automatic, Rapid 20, care and future program codes use the same state
contract. Observations are bounded to 24 hours and remaining time to 1440 minutes. A countdown increase starts
new evidence; a pause, delay, error, unknown phase, disconnected reading, backward clock or gap over 12 minutes
disarms the detector. Resuming requires fresh progress. These conservative rules can miss a completion.
Phase definitions follow the community Home Assistant hOn mapping:
https://github.com/Andre0512/hon/blob/main/custom_components/hon/const.py (`MACH_MODE`, `DISHWASHER_PR_PHASE`).
The prewash countdown is physically sampled; other programs have automated state-machine coverage, not yet
physical end-to-end validation. A cloud completion is not proof of clean dishes, an opened door or fresh telemetry.

Automated tests cover real-core/MCP delivery, multiple source completions, acknowledgments, restart/replay,
foreign-session rejection, storage recovery and failure cases. The physical wash-to-agent path remains pending.
No device command is triggered by this observer. Controls in CONTROLS.md are separate explicit actions, never
consequences of an event. Historical `hon_monitor_poll`/Flow preview assets are not the managed setup path.

## Español

Los avisos vienen apagados y se eligen aparte de conectar: charla personal y horario de silencio. APX consulta
cada cinco minutos en reposo y cada minuto durante cualquier programa reconocido, sin llamar al modelo por consulta.
No requiere un Flow ni pegar rutas. La herramienta queda reservada al monitor; una llamada ordinaria no puede
reiniciar su sesión. Guarda hasta 1000 finales individuales, devuelve los diez más antiguos y los retira sólo
cuando APX reconoce recibos persistidos. Dos ciclos no se pisan. Si se llena, conserva los anteriores e informa
la presión; un ciclo nuevo puede perderse si sus señales terminan antes de recuperar espacio.

Desactivar cancela pendientes; apagar la extensión también revoca el permiso de avisos y reconectar no lo activa.
Cambiar la configuración reinicia deliberadamente el detector y su cola. Un reinicio del core no. APX retiene
recibos terminales 24 horas y pendientes hasta siete días, con límites de 10.000 recibos y 60 nuevos por minuto
por instalación. Compacta el historial vencido; fuera de esa ventana una repetición puede admitirse como nueva.

El detector cubre todos los códigos numéricos de programa (0–65535) del modelo Candy CF 6C4F1PS-12, sin lista
cerrada de nombres. Exige modo 2, fase 1/2/3/4/6 y progreso del tiempo, seguido por modo 7/fase 5 con tiempo cero
en dos lecturas separadas por al menos un minuto. El código debe mantenerse; un cambio reinicia la evidencia.
El límite es de 24 horas por observación y 1440 minutos restantes. Un aumento del tiempo reinicia la evidencia;
pausas, demoras, errores, desconexiones, fases desconocidas, reloj hacia atrás o huecos mayores a 12 minutos
la descartan. Reanudar requiere nuevo progreso. Estas reglas conservadoras pueden perder una finalización.
El mapeo de fases proviene de Home Assistant hOn (enlace arriba). Sólo el prelavado tiene muestras físicas;
el resto tiene cobertura automática del detector, no validación física. Puede perder ciclos entre consultas
y la nube puede estar desactualizada. El monitor no acciona el equipo ni manda mensajes de canal por su cuenta.
La prueba física completa de entrega automática sigue pendiente; los arranques explícitos se explican aparte.

Docs impact: updated the bilingual monitor contract, managed lifecycle, privacy, acknowledgments, bounded
retention/recovery and remaining physical-validation limits.
