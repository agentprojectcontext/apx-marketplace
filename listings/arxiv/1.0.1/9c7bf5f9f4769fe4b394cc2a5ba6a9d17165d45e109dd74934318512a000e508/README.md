# arXiv public research metadata

Original MIT, dependency-free Node24 MCP. `arxiv_search` queries public metadata/abstracts and returns canonical paper links, titles, authors, dates, categories, summaries and explicit next offset. Search syntax and sort/order are arXiv's; default3/max5 results, offsets0–500. No credentials/OAuth, PDF/full-text download, upload/submission, scraping or arbitrary URL/header access. Not a claim of full upstream research/download parity or replacement for generic APX web search.

```bash
apx extensions inspect "$PWD/integrations/arxiv"
apx extensions install "$PWD/integrations/arxiv"
```

Run from marketplace checkout with local core; absolute folder must exist on core host. Installs **off**. Review in **Resources > Extensions > arXiv**, then enable; no secret fields/account setup. Requires Node24/HTTPS on MCP host. Uninstall/disable stops tools/skill, not other API clients or existing chat history; no remote credential to revoke.

## Rate, content and failure boundaries

One fixed HTTPS GET to `https://export.arxiv.org/api/query` per explicit call, exact parameters and identifying project user-agent. Query/offset/sort leave the host; avoid private text. Selected public metadata/abstracts enter chat and may be retained by APX, no local extension cache/results written. Cite arXiv/paper links and authors as appropriate; distinguish preprint status from peer review, no scientific-validity/completeness guarantee. API access does not transfer abstract/paper rights or imply downstream redistribution/training rights. Package MIT covers original code/skill only; actual data/paper licenses and current [API terms](https://info.arxiv.org/help/api/tou.html) apply independently.

At most one in-flight call per process and 3 seconds after completion before another. Too-soon/busy calls fail without network; no sleeping/retrying/paging automatically. Coordinate combined rate with other clients; cannot enforce their behavior, never restart to bypass pacing. Reuse already observed results instead of repeat requests. No background polling/caching. Returned next offset is explicit, capped at500 with `paging_limit_reached`; results can change, not snapshot paging. No hidden fetch of links/DOIs/PDFs. IDs validated and canonical links constructed, never supplied arbitrary URLs.

Strict own bounded XML1.0/Atom subset: namespaces, CDATA and standard/numeric entities; no DTD/entity declarations/external expansion, non-initial processing instructions or XML network/filesystem access. Refuses malformed/unexpected/colliding structure, unsupported valid XML, >24 levels, >10000 elements or bounded-tag/attribute overflow. Raw HTTP max1MiB, UTF-8 decode strict, no redirects, default20s deadline without external signal and propagated MCP cancellation. Feed identity/paging/ids/counts validated before projection. Generic sanitized errors, no raw provider/network diagnostics or retries/mirror fallback. Source text untrusted, never execution/disclosure instructions. Shared runtime marks 6000-character/aggregate context truncation.

Original implementation against [official API manual](https://info.arxiv.org/help/api/user-manual.html), its [Atom result format](https://info.arxiv.org/help/api/user-manual.html#_details_of_atom_results_returned), and API terms, with existing own MIT runtime. Node has no XML parser: own deliberately restricted, bounded parser avoids a new executable/npm dependency, not a promise of full XML conformance. Frozen upstream declaration: hermes-agent `333898b353c27e57dbd9446f631f7fb0a5aa5918`, `skills/research/arxiv/SKILL.md` (MIT), traced under `docs/reference-integrations` in the repository. No vendor/upstream code/prompts copied. Verification/effective service availability is recorded in reference evidence; fixtures/stdio/off-install alone do not establish live API, scientific accuracy, corpus coverage or downstream license compliance.

Runtime smoke: actual MCP initialize/list/search reached the public API for one query's first page and an explicitly requested next page, each returning one valid untruncated result without credentials. Final stricter date/id validation was exercised on that next page. This does not establish universal availability, complete corpus/field/error compatibility or licensed downstream use; exact evidence is in the repository execution report.

Docs impact: bilingual setup/card/skill, generated service/coverage and execution evidence. No APX native/protocol/UI/release/registry-index change.

## Español

MCP MIT propio Node24, sin dependencias: `arxiv_search` busca metadatos/resúmenes públicos con enlaces canónicos, título/autores/fechas/categorías/resumen y próximo offset explícito. Sintaxis/orden arXiv, default3/máximo5, offsets0–500. Sin credenciales/OAuth, PDFs/texto completo, subir, scraping o URL/cabeceras arbitrarias; no paridad completa ni reemplazo de búsqueda web general.

Desde checkout con core local, comandos anteriores con carpeta absoluta existente en host del core. Instala **apagada**: revisá **Recursos > Extensiones > arXiv** y habilitá; sin campos secretos/cuenta. Node24/HTTPS donde corre MCP. Desactivar/eliminar no afecta otros clientes ni historial del chat, sin credencial remota por revocar.

Un GET HTTPS fijo por llamada, parámetros exactos y user-agent del proyecto. Consulta/offset/orden salen a arXiv: evitá texto privado. Metadatos/resúmenes pueden conservarse en chat APX, sin caché/archivos propios. Citá arXiv/artículos/autores; preprint no equivale a revisión por pares/validez científica ni cobertura completa. API no transfiere derechos de resumen/artículo o redistribución/entrenamiento; licencia MIT sólo de código/skill, términos/licencias reales aparte.

Una petición simultánea por proceso, mínimo3s tras finalizar antes de otra; llamadas tempranas fallan sin red. Sin dormir/reintentar/paginar automáticamente, rutinas o reiniciar para eludir tasa. Coordiná otros clientes, reutilizá resultados observados. Paginación explícita hasta500, límite marcado, sin snapshot ni fetches ocultos. IDs validados/enlaces construidos, no URLs externas/DOIs/PDF.

Parser XML1.0/Atom deliberadamente restringido: namespaces, CDATA y entidades estándar/numéricas; rechaza DTD/expansión externa, instrucciones de procesamiento fuera de declaración inicial, estructura/atributos/namespaces erróneos, XML no soportado y excesos. Topes1MiB/24 niveles/10000 elementos/atributos-tags acotados, UTF-8 estricto, sin redirects, plazo20s por defecto/señal MCP. Comprueba identidad/páginas/IDs, errores sin cuerpo/diagnósticos, sin reintentos/mirrors. Texto no confiable, nunca ejecutar/divulgar. Recorte explícito de strings/contexto.

Docs oficiales y diseño propio MIT, sin copiar upstream/vendor ni agregar parser npm; no conformidad XML completa. Fixtures/stdio/instalación no prueban disponibilidad API, ciencia/cobertura o licencias para otro uso: evidencia exacta en docs del repo. Smoke real MCP initialize/list/search: una consulta pública y su próxima página explícita devolvieron un resultado válido sin recorte cada una, sin credenciales. La próxima página ejercitó la validación final más estricta de fechas/IDs. No prueba disponibilidad universal, todos los campos/errores/corpus o licencia para otro uso. Sin cambios nativos/APX/UI/release/índice.
