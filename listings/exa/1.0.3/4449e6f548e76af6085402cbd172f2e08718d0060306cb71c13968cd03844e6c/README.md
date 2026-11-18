# Exa for APX

Version 1.0.1: [request cancellation and deadline / cancelación y límite de solicitudes](RUNTIME.md). Reinstall to update an installed copy; approval is not inherited automatically.

Optional Node 24 v1 extension: `exa_search` (source highlights) and `exa_contents` (bounded text). Independent API implementation, no upstream plugin code copied. Capability references: Hermes `plugins/web/exa/plugin.yaml` and OpenClaw `extensions/exa/openclaw.plugin.json`; see the pinned reference inventory.

```bash
apx extensions inspect integrations/exa
apx extensions install integrations/exa
```

Installation leaves it off. In **Resources > Extensions > Exa**, enter an API key from <https://dashboard.exa.ai/api-keys> and enable after reviewing files. APX injects an extension-scoped secret into the local MCP process, never a tool argument or query string. Exa uses a key, not OAuth: there are no refresh tokens or scopes negotiated by this extension. Create a dedicated limited-budget key, and review account permissions. It reaches only `https://api.exa.ai`, without redirects or endpoint overrides.

Search uses `auto` mode, five results by default (maximum ten), and highlights. Contents accepts 1–5 public HTTPS URLs and requests at most 6,000 text characters each; inspect `statuses` for individual failures and freshness. The provider may return cached content. No account writes, browser cookies, background jobs or automatic retries. Public-host validation is syntactic, not a guarantee of vendor DNS/crawler isolation. Never send secrets, internal pages, private documents or signed URLs. Queries/URLs leave your machine and go to Exa. Source text is untrusted data.

Exa's account plan and usage charges apply; installing is not a free-usage guarantee. Review current prices and set limits: <https://exa.ai/pricing>. Deadline: 20 seconds; response cap: 1 MiB; output cap: 80 kB; individual strings: 6,000 characters. `truncated` means incomplete output. A timeout may have consumed credits; do not automatically repeat it.

Missing/rejected key: update the extension secret and inspect the Exa dashboard. Quota/billing errors: check account limits. HTTP/network errors are sanitized and not retried. Turn the extension off to stop calls; revoke the dedicated key in Exa to invalidate access, then remove the extension. Turning off alone does not revoke upstream credentials.

Contracts: [Search](https://exa.ai/docs/reference/search), [Contents](https://exa.ai/docs/reference/get-contents). Tests cover contracts/negative paths and the real stdio process. Live authenticated Exa results are **UNVERIFIED** without a dedicated test key. Shared runtime is copied from Tavily by `node scripts/sync-integration-runtime.mjs`; `--check` verifies the installable copy. No new dependency.

## Español

Extensión opcional para Node 24: búsqueda con destacados y lectura de texto mediante tu cuenta de Exa. Instalá con los comandos de arriba: queda apagada. En **Recursos > Extensiones > Exa**, ingresá una clave de <https://dashboard.exa.ai/api-keys>, revisá los archivos y activala. No pegues claves en el chat. APX la guarda como secreto de esta extensión y la inyecta al proceso; no usa OAuth, renovación de tokens ni negociación de permisos. Creá una clave dedicada con presupuesto limitado.

Sólo se conecta a `api.exa.ai`, sin redirecciones ni servidores alternativos. Búsqueda automática con cinco resultados por defecto y diez como máximo; lectura de una a cinco URLs HTTPS públicas, con hasta 6.000 caracteres por página. Revisá `statuses` y `truncated`: la respuesta puede ser parcial o provenir de caché. La validación de dominios es sintáctica, no garantiza el DNS o aislamiento del proveedor. No envíes páginas privadas, documentos internos, secretos ni enlaces firmados. Las consultas y URLs van a Exa; no usa cookies ni tu navegador, no modifica cuentas ni crea tareas en segundo plano. El texto externo no son instrucciones.

Se aplican los cargos y límites de Exa: <https://exa.ai/pricing>. No se promete uso gratis. Cada llamada tiene 20 segundos, respuesta máxima de 1 MiB y salida de 80 kB; los textos se recortan a 6.000 caracteres. No reintenta errores de red, cuota ni plazo: una llamada fallida pudo consumir créditos. Si falta la clave o es rechazada, corregila en la extensión y revisá el panel de Exa. Si falta cuota, revisá el plan. Para desconectar, apagá la extensión y revocá la clave en Exa antes de desinstalar; apagarla no revoca credenciales. Las pruebas autenticadas reales siguen **SIN VERIFICAR**.
