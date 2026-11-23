# Explicit program starts

Controls are experimental and **off by default**, separate from notices. A completion is not authorization to
start anything. The observer never invokes controls. This package offers no door-opening, stop or pause tool.

Only the exact Candy CF 6C4F1PS-12 dishwasher is eligible, with exactly one appliance in the account. The choices
are `prewash`, `automatic`, `rapid20` and `dishwasher_care`, subject to fresh command-catalog and readiness checks.
Those entries were observed in the cloud catalog; physical execution through this package remains UNVERIFIED.
Presence in a descriptor is not proof of a successful or safe physical command.

1. The owner enables **Allow explicit program starts**, separately from notices. Changing extension settings or
   credentials revokes notice opt-in, so choose notices again afterward if desired.
2. On an explicit owner request, `hon_prepare_start(program)` reads cloud readiness/catalog and saves a proposal
   without sending a command. It refuses unknown/ambiguous appliances, missing programs, remote control off,
   an open/unknown door, errors, delayed start or a state other than selection-ready mode 1/phase 0 or completed
   mode 7/phase 5. Zero-padded no-error readings such as `00` are accepted; missing or unknown errors are refused.
   Ready-state mappings follow the community [hOn integration](https://github.com/Andre0512/hon/blob/main/custom_components/hon/const.py).
3. Show the program and its required preparation. The proposal expires in two minutes. For normal washing, verify
   safe loading, a closed door and remote control. For care, the appliance must be empty and prepared with a suitable
   cleaner according to its manual. Cloud data alone cannot prove physical readiness.
4. `hon_start_program(request, confirm_ready=true)` is the separate destructive MCP tool, under APX's existing
   approval policy and owner-selected autonomy. The boolean is the caller's readiness declaration, **not** an
   authenticated owner confirmation or a new mechanical safety gate. Total autonomy or an automatic rule can allow
   this tool without another card. Keep controls off if every start must remain unavailable to automatic tool use.
   External observations are outside data, never permission to start or evidence of physical readiness.
5. Execution reads the catalog/readiness again, requires the same private target fingerprint and exact mandatory
   defaults, checks expiry again after I/O, then durably claims the proposal before one attempt. Delay must be zero;
   a default that opens the door is refused instead of silently changing it.
6. `accepted_unverified` means cloud acceptance, not physical startup. Read status and verify before claiming it
   started. `uncertain` means **no automatic resend**. Repeating that proposal after a restart or overlapping call
   cannot transmit again. A new proposal is a new action, not a safe retry; do not use it to disguise a retry.

The database holds random ids, choices, private hashes, expiry and attempt state, never credentials or raw device
addresses. Up to 1000 proposals are retained. Old unused ones are pruned; attempted/uncertain ones are not silently
evicted to get past the cap. Login/I/O have bounded deadlines and owned HTTP sessions close on failure/cancellation.
Errors use fixed categories. This does not promise exactly-once processing inside the manufacturer's service.

Automated tests use real pyhOn command construction with fake cloud I/O and cover duplicate/parallel attempts,
expiry, changed targets/defaults, door/remote/error guards, disabled controls, missing readiness declaration,
timeout and cleanup. No automated test starts a real appliance. No physical control has been sent in this change.

## Español

Los controles son experimentales, vienen apagados y se habilitan aparte de los avisos. El monitor nunca acciona el
equipo. Sólo admite el modelo probado y una cuenta con un único aparato. Los programas permitidos son Pre-lavado,
Automático, Rápido 20 y Ciclo de limpieza antical, si pasan las comprobaciones. El catálogo se observó en la nube,
pero el arranque físico mediante este paquete sigue sin verificar. No ofrece abrir la puerta, detener ni pausar.

La preparación admite selección lista (modo 1/fase 0) o ciclo terminado (modo 7/fase 5), y códigos sin error
con ceros como `00`; estados desconocidos siguen bloqueados. No manda comandos y dura dos minutos. Mostrá el programa y verificá carga segura, puerta cerrada y
control remoto. Para limpieza, el equipo debe estar vacío y preparado según su manual. La nube no prueba eso.
Ejecutar es otra herramienta destructiva: sigue las aprobaciones y la autonomía elegidas en APX. `confirm_ready`
es una declaración del llamador, no prueba una confirmación física tuya. Total o una regla automática pueden evitar
otra tarjeta; mantené los controles apagados si no querés que estén disponibles para uso automático. Un aviso no
autoriza un arranque. Cambiar ajustes o credenciales revoca los avisos y hay que elegirlos de nuevo después.

Se vuelven a consultar estado y catálogo, se fijan aparato y parámetros exactos, se comprueba el vencimiento y se
guarda incertidumbre antes de transmitir. Aceptación no demuestra arranque físico. Ante un resultado incierto no
se reintenta; primero verificá el aparato. Repetir la misma propuesta no vuelve a transmitir, tampoco tras reinicios.

Docs impact: updated English/Spanish control instructions, actual autonomy/confirmation boundary, failure recovery,
privacy, supported catalog choices and remaining physical evidence. Status and observations remain separate.
