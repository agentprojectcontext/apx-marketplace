# Tavily for APX

Version 1.0.1: [request cancellation and deadline / cancelación y límite de solicitudes](RUNTIME.md). Reinstall to update an installed copy; approval is not inherited automatically.

Optional, dependency-free v1 extension with `tavily_search` and `tavily_extract`. Requires Node 24 and a Tavily account/API key. This is an independent API implementation, not copied from an upstream plugin. Reference capabilities: Hermes `plugins/web/tavily/plugin.yaml` and OpenClaw `extensions/tavily/openclaw.plugin.json`; pinned SHAs are in `docs/reference-integrations/declarations.json`.

## Install and connect

From this repository root:

```bash
apx extensions inspect integrations/tavily
apx extensions install integrations/tavily
```

Installation leaves it **off**. In **Resources > Extensions > Tavily**, enter the API key from <https://app.tavily.com/home> and turn it on after inspecting the files. The key is stored using APX's extension-owned secret mechanism and injected into the MCP process, not placed in tool arguments. It is never a query-string parameter. The extension does not implement OAuth or refresh tokens: this provider uses an API key. Only HTTPS requests to `api.tavily.com` are made; there is no configurable proxy or base URL.

Once on, ask an agent to search with Tavily; it can discover tools through `tool_search`. Inspect the source citations. `tavily_extract` returns both successful and failed URL results. The extension caps requests, timeouts and output; `truncated: true` means the output is not complete.

## Permissions, cost and privacy

Read-only tools, basic-depth search/extraction only. The key grants API usage and can incur charges: set account usage limits before enabling. There is no APX charge for installing; Tavily's plan, credits and current prices apply: <https://docs.tavily.com/documentation/api-credits>. No free-tier entitlement or specific price is promised. Queries and page URLs are sent to Tavily; extracted pages come from Tavily, not the owner's browser or cookies. Public HTTPS hostname checks are syntactic; they are not an independent guarantee of the vendor's DNS or crawler isolation. Never submit private URLs, credentials or signed links.

No writes, account modification, browser access, background jobs or automatic retries. Redirects from the API are refused. Each API request has a 20-second deadline, a 1 MiB response cap and an 80 kB output cap. Text fields are truncated at 6,000 characters. Vendor errors are not forwarded verbatim. Untrusted page content remains untrusted.

## Errors and disconnect

- Missing key: enter `api_key` in the extension screen; do not paste it into chat.
- Authentication rejected: replace the key there after checking the provider dashboard.
- Quota/billing limit: check usage and limits; no retry was made.
- Timeout/network error: the request might have consumed credits. Do not automatically repeat it.
- Partial extraction: check `failed_results`; HTTP success does not mean every page was read.
- To stop: turn the extension off. To revoke upstream access: delete/revoke its key in Tavily, then remove the extension. Turning off APX alone does not revoke a vendor key.

## Verification and API contract

Official contracts consulted: [Search](https://docs.tavily.com/documentation/api-reference/endpoint/search), [Extract](https://docs.tavily.com/documentation/api-reference/endpoint/extract). Tests exercise request parameters, error handling, bounds and stdio negotiation without a paid account. Live authenticated provider results are **UNVERIFIED** until a dedicated account key is supplied through the extension screen. Do not confuse mocked API tests with live validation.

## Español

Extensión opcional para buscar con Tavily y extraer texto de páginas públicas. Necesitás Node 24, una cuenta y una clave de Tavily. Instalá con los comandos de arriba; queda **apagada**. En **Recursos > Extensiones > Tavily**, ingresá la clave de <https://app.tavily.com/home> y activala después de revisar los archivos. No pegues claves en el chat. Se guarda como secreto de la extensión y se inyecta al proceso; no usa OAuth ni renovación de tokens.

Las consultas y URLs se envían a Tavily y consumen los créditos de tu cuenta, incluso si una respuesta tarda demasiado. Configurá límites de gasto; consultá los precios vigentes en <https://docs.tavily.com/documentation/api-credits>. No se promete un plan gratis. No accede a cookies ni al navegador, ni inicia sesión ni modifica cuentas. Acepta nombres de dominio HTTPS públicos, sin credenciales ni puertos especiales: es una validación sintáctica, no una garantía sobre el DNS o el aislamiento del proveedor. No envíes enlaces internos, privados, firmados ni secretos.

La búsqueda usa profundidad básica, cinco resultados por defecto y diez como máximo; la extracción admite de una a cinco URLs. Sólo se conecta a `api.tavily.com`, sin redirecciones ni reintentos automáticos. Tiene un plazo de 20 segundos, un máximo de respuesta de 1 MiB y salida de 80 kB; recorta textos a 6.000 caracteres. Revisá `failed_results` y `truncated`: una respuesta exitosa puede ser parcial. El contenido externo no son instrucciones.

Si falta la clave o es rechazada, corregila en la pantalla de la extensión y revisá el panel de Tavily. Si aparece un límite de cuota, revisá el plan. Un error de red o plazo vencido no se reintenta y pudo consumir créditos. Para desconectar, apagá la extensión; para revocar el acceso, eliminá la clave en Tavily y después desinstalá. Apagar APX no revoca una clave del proveedor. Las pruebas autenticadas reales siguen **SIN VERIFICAR**; las pruebas automatizadas no usan una cuenta paga.
