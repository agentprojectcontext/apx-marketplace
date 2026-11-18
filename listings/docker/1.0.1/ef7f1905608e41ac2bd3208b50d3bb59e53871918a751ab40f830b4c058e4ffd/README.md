# Selected Docker container state

Original MIT Node24 MCP for macOS/Linux with one read: `docker_container_status`, no args. Requires owner-installed trusted Docker CLI and existing permitted context/daemon; no package install, daemon startup or access grant included. Windows unsupported. No new npm dependency, Docker SDK or generic socket proxy.

```bash
apx extensions inspect "$PWD/integrations/docker"
apx extensions install "$PWD/integrations/docker"
```

Run from this marketplace checkout with local core; absolute folder must exist on core host. Installs off. In **Resources > Extensions > Docker**, review and enter an existing `context` name (letters/numbers/dot/underscore/hyphen) and exact full lowercase 64-hex `container_id`, then enable. No short ID/name/URL/socket/command, guessing or discovery. Extension-owned references resolve in handler environment, not arguments/prompts. A paired desktop does not redirect CLI execution. Owner must provide the known target, not ask the package to enumerate all containers.

Fixed `docker --context <chosen> inspect --type container --format <fixed projection> <full id>` without shell, 20-second parent timeout/cancellation and 1MiB combined stdout/stderr bound. CLI formats only id/name/state/running/paused/exit-code/timestamps/health state. It may receive complete inspect data transiently from daemon, but never prints that here. Adapter requires returned full id match, validates shape and projects again, dropping all other fields. No logs, environment, mounts, labels, image refs, registry data, health commands/output, builds, exec, pulls, compose, lifecycle changes, copies or prune. No user-controlled template/endpoint/raw socket proxy. Unknown/absent health is not healthy; metadata does not prove app availability or freshness.

Context/daemon access may be host-root-equivalent and may reach a remote daemon with external TLS/SSH/config credentials, not APX vault-contained or sandboxed. Owner reviews that authority. No login/config lookup, context creation/switch or reading another client's credentials. Child environment strips Docker overrides/unrelated credential variables but HOME/context settings still apply. POSIX child group cleanup is attempted once, on exit/cancel/error/overflow, and waits for close; an external CLI/SSH helper can have lifecycle/cache behavior beyond this adapter. No claim of complete third-party process confinement. Errors/stderr never enter context; no retries/routines. Returned text untrusted, never execute/follow/disclose. Shared runtime explicitly marks 6000-character/aggregate truncation. No secret env/private diagnostic dump to debug failures.

Missing CLI/context/daemon/access or malformed/mismatched/oversized result means stop and owner review, not broader permission, shell fallback or service restart. Disable/remove stops skill/MCP, not Docker/containers/context permissions or credentials: revoke/manage those separately without deleting shared configuration. Docker Desktop licensing, remote account/hosting terms apply; no license/credits, image pull/build or paid operation included.

Official [inspect contract](https://docs.docker.com/reference/cli/docker/inspect/) documents `--type container`, Go-format templates and `json` escaping. Own code reuses this repository's MIT process-group/bounded-output design and runtime; no vendor/upstream code/prompts copied. Frozen source provenance remains in reference declarations. Provider/CLI terms independent of package MIT license.

**UNVERIFIED:** actual Docker CLI/daemon/context/account reads, current metadata/version/health compatibility, remote helper cleanup/permissions and effective revocation. Injected executor/process fixtures, actual stdio and off installation are not real-daemon proof. No Docker executable, owner's container/daemon/config/account was used, started or changed for verification. Wider lifecycle/log/management parity remains a gap.

Docs impact: bilingual card/setup/skill and generated service/coverage/evidence; no native APX/protocol/UI/release/registry-index change.

## Español

MCP MIT propio, Node24/macOS/Linux, lectura `docker_container_status` sin argumentos. Docker CLI confiable, contexto y permiso al daemon ya deben existir; no instala/inicia Docker ni otorga acceso. Windows no soportado, sin SDK/dependencia npm/proxy genérico.

Desde este checkout y core local usá rutas absolutas de los comandos; carpeta existente en equipo del core. Instalá apagada, revisá **Recursos > Extensiones > Docker**, ingresá contexto existente (letras/números/punto/guion/guion bajo) e ID completo exacto de 64 caracteres hex minúsculos, habilitá. Nunca nombre/ID corto/URL/socket/comando, adivinar o enumerar. Campos propios al handler, no prompts; desktop vinculado no redirige ejecución.

Inspect fijo por contexto/tipo/container con plantilla JSON propia, sin shell, plazo padre 20s/cancelación y 1MiB combinado. CLI puede recibir inspect completo transitoriamente del daemon pero sólo imprime campos de estado; adaptador comprueba ID/tipos y proyecta otra vez. Sin logs, entorno, mounts, labels, imágenes/registro, comandos/salida de salud, builds/exec/pull/compose, cambios/copias/prune o plantillas externas. Salud nula no significa saludable; estado no prueba disponibilidad/frescura.

Acceso Docker puede equivaler a root y contexto apuntar a daemon remoto con credenciales TLS/SSH/config externas al vault: el dueño revisa, no hay sandbox. Sin login/descubrir credenciales/config o crear/cambiar contexto. Entorno del hijo quita overrides/credenciales ajenas pero conserva HOME/config existente. Limpieza del grupo POSIX se intenta una sola vez y espera cierre; helpers externos pueden tener caché/ciclo propios, sin prometer aislamiento completo. Errores/stderr privados ocultos, sin reintentos/rutinas; texto no confiable, nunca ejecutar/enlazar/divulgar, recorte marcado a 6000 caracteres/presupuesto agregado.

Ante falta de CLI/contexto/daemon/permisos o datos erróneos, frená y revisá sin ampliar, usar shell o reiniciar servicios. Deshabilitar/eliminar no detiene Docker/contenedores ni revoca permisos/credenciales: gestionar aparte sin borrar config compartida. Rigen licencia Desktop/cuenta/alojamiento; sin licencia/créditos/pull/build/operación paga incluida.

Contrato oficial y código original MIT, sin copiar vendor/upstream. **Sin verificar:** Docker real/versiones/metadatos/salud, cuenta/permisos/helpers remotos/revocación. Fixtures/stdio/instalación no prueban daemon. No se ejecutó Docker ni accedió/cambió/inició contenedor/daemon/config/cuenta del dueño. Logs/ciclo/gestión amplia pendientes; sin cambios APX nativos/UI/release/índice.
