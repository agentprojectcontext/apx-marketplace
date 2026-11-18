# Contrato del índice oficial

Endpoint: `https://marketplace.apxagent.app/index.json`.

`index.json` usa **schema 1**, se sirve por HTTPS y deliberadamente **no tiene firma**. Sólo lista entradas aprobadas por APX; no admite publicación de terceros. Los archivos no listados no integran el catálogo oficial. La confianza depende de este endpoint HTTPS oficial, no de una firma ni de un checksum bajado de cualquier espejo.

De cada entrada se leen `id`, `kind`, `name`, `description`, `categories`, `tier`; de cada versión:

- `version`: versión semántica.
- `apx`: versión mínima de APX.
- `content_hash`: 16 caracteres hexadecimales minúsculos, la huella de archivos de APX.
- `archive.url`: URL HTTPS sin credenciales del ZIP `.apxext` independiente en este repositorio.
- `archive.sha256`: 64 caracteres hexadecimales minúsculos, SHA-256 de los bytes exactos descargados (sin prefijo `sha256-`).

Las entradas con ficha rica también exponen:

- `tagline`: descripción corta en inglés, con su traducción en la ficha.
- `category`: uno de `productivity`, `communication`, `docs-notes`, `files`, `development`, `data`, `design-media`, `sales-customers`, `finance`, `marketing`, `devices-home`, `search-research`, `business-systems`. El singular es distinto del campo anterior `categories`, de categorías generales.
- `listing`: URL HTTPS absoluta del `apx-listing.json` de esa versión.
- `icon`: URL HTTPS absoluta de su SVG o PNG de 256 px.

La ficha schema 2 contiene descripciones largas bilingües, tres pedidos por idioma, conexión, enlaces, publicador y leyendas de imágenes. Se permite `screenshots: []` hasta tener una imagen adecuada. Las conversaciones ficticias con estilo APX y rótulo explícito son ilustraciones, nunca prueba de una cuenta real; no deben inventar la interfaz del servicio. La versión de la ficha es independiente de la del manifest ejecutable.

Schema 1 puede incluir otros campos de publicador, ficha, origen e integridad. Los consumidores ignoran campos desconocidos. Si `schema` supera el soportado, se ignora todo el índice y se pide actualizar APX. Los campos conocidos obligatorios se validan antes de usarlos.

Verificá SHA-256 antes de extraer el archivo. Después de una extracción segura, calculá la huella APX: ordená las rutas relativas con el orden de strings predeterminado de JavaScript; para cada archivo agregá a un único SHA-256 su ruta UTF-8, un byte NUL, sus bytes y otro byte NUL. `content_hash` son los primeros 16 caracteres hexadecimales del resultado. Las rutas son relativas y usan `/`. Recomprimir puede cambiar `archive.sha256` sin cambiar `content_hash`.

La procedencia oficial requiere coincidencia de **id + version + content_hash** con el índice. El hash del ZIP por sí solo no otorga ese estado. Descargar no autoriza habilitar una extensión: siguen vigentes la instalación y aprobación de APX. Este contrato no afirma que todas las versiones publicadas de APX ya consuman el índice o instalen archivos.

Los archivos con nombre por hash son inmutables. La procedencia identifica los commits fuente originales, cuyo repositorio pasó a llamarse `apx-marketplace-private`; su historial privado no se publica acá.

[English](INDEX.md)
