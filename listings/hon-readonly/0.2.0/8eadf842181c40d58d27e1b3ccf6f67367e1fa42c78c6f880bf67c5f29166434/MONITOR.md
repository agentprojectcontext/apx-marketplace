# Prewash completion observation (local preview)

The optional monitor is **off by default**. It never starts, pauses, stops or opens an appliance. It does not register a timer or send a message by itself. Its cloud mapping is supported by one physically observed prewash cycle on **Candy CF 6C4F1PS-12** only. This is not generic hOn completion detection.

## Configure

On the extension page, enable **Observe prewash completion** and choose an existing, private **Monitor state folder**. The MCP process needs write access to that folder. Turning this option on permits local observation state, not appliance controls. Leave it off until the complete scheduled delivery path has been tested.

`hon_monitor_poll` performs one cloud read and stores a small SQLite state in `hon-monitor.sqlite` in that folder. It returns `{event: null, state}` for ordinary polls and a bounded `cycle_finished` event once for a qualifying cycle. It is deliberately not marked read-only: it writes local state. Missing configuration and storage failures return fixed error categories without private paths or exception bodies. The ordinary `hon_status` tool does not write state.

The monitor refuses ambiguous accounts: exactly one returned appliance, no skipped/truncated results, the exact known model and appliance type, connected state and complete numeric readings are required. It does not save names, credentials, tokens, MACs, serials or raw telemetry. The stored state is a random cycle identifier, timing, countdown and qualification flags. This still describes household activity; protect the folder. Changing accounts while monitoring is enabled is unsupported: disable monitoring and use a different empty state folder before changing accounts.

## Recognition and limits

- PREWASH only: program 12, no delay and no reported errors.
- At least two spaced running readings (mode 2, phase 1) with a decreasing positive countdown, followed by two spaced completion readings (mode 7, phase 5, zero minutes).
- Samples less than 60 seconds apart do not qualify. Gaps over 12 minutes, backwards clock movement, unknown readings, errors, disconnection, a different program or a pause/off state disarm the sequence. The observation window is at most 30 minutes.
- An installation starting at idle/finished cannot notify for an old cycle. A repeated completion or process restart does not emit the same event again. A countdown increase starts qualification again.
- No device timestamp is available. A changing countdown and the observed terminal sequence provide continuity evidence, **not a guarantee of live appliance state**. Cloud replay, an account change, unusual firmware or other untested behavior cannot be ruled out. Notifications must say that the cloud reports completion, not assert a safety condition. Do not use this for unattended controls.
- A five-minute schedule can miss short cycles or their qualifying readings. Two completion observations add roughly five to ten minutes of delay after the first terminal cloud reading. Missing evidence deliberately means no notification.

## Schedule without a model on every poll

Use an explicitly configured APX Flow: **schedule → command → condition → agent or notification**. The extension cannot install/publish this Flow automatically. APX's schedule survives restarts and allows a minimum five-minute interval. A routine that calls a model on every tick is not the implementation.

The shipped `monitor-poll.mjs` adapter runs the installed `apx mcp run hon-readonly-status hon_monitor_poll '{}'` command through the ordinary APX protocol client. It needs Node 24 and the `apx` executable on the command step's PATH. No credentials are passed through command text or copied to Flow variables. Point the fixed Flow command at the installed adapter file; do not reference a temporary build folder. Use a 90-second command deadline and `continueOnError: false`. The condition tests command `stdout` for **not empty**. Only a new completion claim writes stdout; disabled/unarmed/ordinary observations are empty. Errors fail the command. On the matching branch, use a fixed owner-selected destination or agent with text such as “The hOn cloud reports that prewash finished.” The Flow's own publish/enable confirmation still applies.

**Delivery semantics:** the SQLite claim commits before the MCP result is returned, so ordinary retries, parallel polls and restarts do not duplicate the claim. It is at-most-once, not exactly-once delivery: a crash after commit, a failed adapter, or a failed downstream Flow action can lose the notice. There is no automatic resend. Reconcile a failed send rather than deleting state or resetting the monitor to force a resend. A future durable acknowledged outbox is needed for reliable retries across this boundary.

To stop scheduling, disable the Flow; to stop observation, also turn off the monitor setting. Turning off the extension does not disable an independently created Flow: its command will fail while the MCP is unavailable. Keep the state file across upgrades/restarts. Deleting it loses deduplication history and requires observing a new qualifying running sequence. Remove it only when all monitor commands have stopped and you explicitly want to discard that history.

## Verification status

The observed real cycle showed a countdown from 11 to 1, then mode 7 / phase 5 / zero minutes sustained across several reads, with human confirmation of completion. The state machine has deterministic replay, negative-path and SQLite restart tests. This does not establish other programs or brands. End-to-end scheduled delivery, installed-package authentication/profile recognition and restart behavior on the running core must be verified separately before enabling this preview for unattended use.

## Español

El monitor es opcional, viene **apagado** y no controla el aparato. Sólo reconoce la secuencia observada de Pre-lavado del Candy CF 6C4F1PS-12: programa 12, cuenta regresiva decreciente en modo 2/fase 1 y dos lecturas finales espaciadas en modo 7/fase 5 con cero minutos. Exige una sola unidad compatible en la cuenta, sin errores, retraso ni datos omitidos. Una pausa, desconexión, programa desconocido o intervalo excesivo desarma la observación. No hay fecha del aparato: la nube puede devolver datos viejos; no es una garantía de seguridad ni se puede generalizar a otros modelos/programas.

En la ficha, elegí una carpeta privada existente para el estado y activá la observación sólo después de probar la entrega completa. hon_monitor_poll guarda un estado SQLite mínimo, sin claves ni identificadores privados, y no manda mensajes por sí mismo. hon_status sigue sin persistir estado. No cambies de cuenta con el monitor activo; apagalo y usá otra carpeta vacía antes de cambiarla.

La programación se configura explícitamente con un Flow: horario cada cinco minutos o más → comando del adaptador monitor-poll.mjs instalado → condición stdout no vacío → agente o aviso a un destino fijo elegido por el dueño. El adaptador necesita Node 24 y apx en PATH; usa el cliente APX normal, sin copiar claves al Flow. Configurá 90 segundos de límite y no continuar ante errores. Publicar/activar el Flow conserva su confirmación habitual. No hay llamadas al modelo en consultas sin un evento nuevo. La cadencia puede perder ciclos cortos y suma unos cinco a diez minutos al primer dato final. Sin evidencia suficiente, no avisa.

La deduplicación sobrevive reinicios, pero la entrega es **como máximo una vez**: si falla después de confirmar el evento en SQLite, el aviso puede perderse. No hay reenvío automático ni garantía de entrega exactamente una vez. Revisá los envíos fallidos; no borres estado para forzar un reenvío. Para detenerlo, apagá el Flow y el ajuste del monitor; quitar la extensión no apaga un Flow independiente. Conservá la base entre reinicios y actualizaciones. Borrarla pierde el historial y exige una nueva secuencia válida. La entrega programada completa sigue sin verificar; esta vista previa no debe activarse para uso desatendido hasta probarla.

Docs impact: updated — this guide documents configuration, storage, cloud freshness, delivery loss, schedule lifecycle and unverified runtime boundaries in both languages. No core/protocol changes or new dependencies.
