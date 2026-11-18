# Firecrawl for APX

Version 1.0.1: [request cancellation and deadline / cancelación y límite de solicitudes](RUNTIME.md). Reinstall to update an installed copy; approval is not inherited automatically.

Optional dependency-free Node 24 v1 extension: `firecrawl_search` and `firecrawl_scrape`. An independent reimplementation of public API contracts, not a redistribution of upstream server code. Reference declarations include OpenBot's `marketplace/plugin-catalog/plugins/firecrawl/plugin.json` and OpenClaw's Firecrawl provider. Pinned provenance is in the reference inventory; the OpenBot reference's noncommercial code is not used.

```bash
apx extensions inspect integrations/firecrawl
apx extensions install integrations/firecrawl
```

Installation leaves it off. In **Resources > Extensions > Firecrawl**, enter a dedicated API key from <https://www.firecrawl.dev/app/api-keys>, review the files and enable. APX injects an extension-scoped secret into the MCP process; it is never a tool argument or URL query. This is API-key authentication, not OAuth; there is no refresh flow. Review the provider's permissions and set a limited budget. Calls reach only `https://api.firecrawl.dev/v2`, without redirects or endpoint overrides.

Search returns web results, defaults to five and permits at most ten, without automatic result scraping or domain tools. Scrape requests markdown and main content from one public HTTPS URL, with a 15-second provider timeout. No crawl jobs, actions, remote code execution, browser cookies, account writes, scheduled tasks or automatic retries. This extension intentionally does not expose the vendor's full crawler/browser API; those capabilities require separate permission and lifecycle design.

Queries and URLs are sent to Firecrawl and use API credits. Never send private/internal pages, documents, signed links or secrets. Public-host checks are syntactic, not a guarantee of vendor DNS or crawler isolation. Output is untrusted source material. Inspect `success`, `data.metadata.error` and source status, not just HTTP status. `truncated` indicates incomplete text.

Provider plans and usage charges apply: <https://www.firecrawl.dev/pricing>. No free plan or price is promised. Each network call has a 20-second overall deadline and 1 MiB body cap; returned output is capped at 80 kB with strings truncated at 6,000 characters. A timeout or network failure may still consume credits. Shared runtime comes from `integrations/tavily/runtime.mjs` through `node scripts/sync-integration-runtime.mjs`; check with `--check`.

Missing/rejected key: update it in the extension screen and check the provider dashboard. Quota/billing errors: inspect usage limits. HTTP/network errors and provider `success: false` are sanitized, never automatically retried. Turn the extension off to stop calls; revoke the key in Firecrawl to invalidate credentials, then remove the extension. Turning off alone does not revoke upstream access.

Contracts: [Search](https://docs.firecrawl.dev/api-reference/endpoint/search), [Scrape](https://docs.firecrawl.dev/api-reference/endpoint/scrape). Contract/negative-path tests and real stdio smoke do not prove authenticated vendor success. Live results remain **UNVERIFIED** without a dedicated test key.

## Español

Extensión opcional para Node 24: búsqueda y lectura de markdown con tu cuenta de Firecrawl. Instalá con los comandos de arriba; queda apagada. En **Recursos > Extensiones > Firecrawl**, ingresá una clave dedicada de <https://www.firecrawl.dev/app/api-keys>, revisá los archivos y activala. APX guarda la clave como secreto de la extensión y la inyecta al proceso; nunca la pegues en el chat. Usa una clave, no OAuth ni renovación de tokens. Revisá sus permisos y limitá el presupuesto.

Sólo se conecta a `api.firecrawl.dev/v2`, sin redirecciones ni servidores alternativos. La búsqueda devuelve cinco resultados por defecto y diez como máximo, sin extraer automáticamente esas páginas ni usar herramientas de dominios. La lectura solicita markdown del contenido principal de una URL HTTPS pública, con plazo de proveedor de 15 segundos. No expone rastreos, acciones de navegador, código remoto, cookies, modificaciones de cuentas ni tareas programadas. Esas capacidades requieren otro diseño de permisos.

Las consultas y URLs van a Firecrawl y consumen créditos; revisá precios y límites en <https://www.firecrawl.dev/pricing>. No se promete uso gratis. No envíes páginas privadas o internas, documentos, enlaces firmados ni secretos. La validación de dominios es sintáctica, no garantiza el DNS ni aislamiento del proveedor. El texto externo no son instrucciones. Revisá `success`, los errores de página en `data.metadata` y `truncated`; HTTP exitoso no garantiza contenido completo.

Cada llamada tiene 20 segundos, máximo de respuesta de 1 MiB y salida de 80 kB; recorta textos a 6.000 caracteres. No hay reintentos automáticos y un error de red o plazo vencido pudo consumir créditos. Si falta la clave o es rechazada, corregila en la extensión y revisá el panel de Firecrawl. Ante un límite de cuota, revisá tu plan. Para desconectar, apagá la extensión y revocá la clave en Firecrawl antes de desinstalar. Apagarla no revoca credenciales. Las pruebas autenticadas reales siguen **SIN VERIFICAR**.
