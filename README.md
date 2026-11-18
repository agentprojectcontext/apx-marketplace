# APX Marketplace

Public distribution of APX's approved standalone extension archives and official catalog metadata. The source workshop stays private. No private repository history, research, configuration, credentials or signing keys are published here.

## Official catalog

The curated index lists **84 packages and 199 versions**, including Android TV and its unchanged development companion APK. The official endpoint is `https://marketplace.apxagent.app/index.json`. The schema 1 index is intentionally unsigned; no signing identity is created. Third-party submissions and public announcements are outside this catalog update.

Every current package has a schema-2 listing in English and Spanish, a 256 px icon, three example requests per language, connection information and official links. The index exposes absolute `listing` and `icon` URLs, a short `tagline` and the listing's singular `category`. Listing schema 2 is separate from the executable manifest version; the incompatible webcam workshop proposal remains unlisted.

The catalog now uses official brand artwork for 78 services on neutral rounded tiles. Six existing symbols remain for documented reasons: five Apple apps (published icon-use restrictions) and DeepWiki (HTTP 429 on direct icon downloads, retained at the owner's request). See [artwork provenance and exceptions](CATALOG-ICONS.md).

Eight listings have imagery: two use real public-page captures, and six have English/Spanish fictional APX-styled conversation examples. Those examples visibly say they are mockups with fictional data, not live connections, and their captions/provenance repeat that distinction. Other listings deliberately have an empty screenshot array until suitable imagery is available. No work-account data is used.

See the [index contract](INDEX.md) ([Español](INDEX.es.md)) for archive SHA-256, APX file fingerprints and provenance. `DISTRIBUTION.json` records the currently listed versions; `SHA256SUMS` covers stored archives. `SOURCE-PROVENANCE.json` retains the initial cohort's historical provenance. Previously published hash-addressed bytes, including unlisted archives, are retained unchanged.

## Using an archive

Archives are ZIP containers under `packages/<id>/<version>/<sha256>.apxext`. Verify exact bytes against the trusted index before safe extraction. Installation leaves an extension off; downloading does not authorize enabling code. Check each version's `apx` minimum: the new rich catalog targets APX 2.16.0, and publishing these files does not release or update APX itself.

Read the package README and skill for supported behavior, accounts, operating-system/device requirements, costs, privacy and limitations. Most live account/device journeys remain unverified; structural tests and successful package inspection are not full vendor parity or live-service proof. No accounts, API keys, credits or paid plans are included. Enter credentials only in APX's approved form/login flow, never in chat. External service and asset licenses still apply, including the documented Open-Meteo data attribution and website-image license.

## Español

Distribución pública de archivos independientes aprobados y metadatos del catálogo oficial. El taller fuente sigue privado: no se publican historial, investigación, configuración, credenciales ni claves de firma privados.

El catálogo usa logos de marca para 78 servicios sobre fondos redondeados neutros. Se conservan seis símbolos por motivos documentados: cinco apps de Apple (restricciones publicadas para usar sus íconos) y DeepWiki (HTTP 429 en la descarga directa, conservado por pedido del dueño). Ver [procedencia y excepciones](CATALOG-ICONS.md).

El índice lista **84 paquetes y 199 versiones**, incluido Android TV con su APK de desarrollo sin cambios. El endpoint oficial es `https://marketplace.apxagent.app/index.json`, schema 1 por HTTPS y deliberadamente sin firma. No se crea identidad de firma ni se habilita publicación de terceros. Esta actualización no incluye anuncios públicos.

Cada paquete actual tiene ficha schema 2 en inglés y español, ícono de 256 px, tres pedidos de ejemplo por idioma, información de conexión y enlaces oficiales. El índice expone `listing` e `icon` absolutos, `tagline` y `category` en singular. La versión de la ficha es independiente de la del manifest ejecutable; la propuesta de webcam incompatible sigue sin listar.

Ocho fichas tienen imágenes: dos capturas públicas reales y seis pares de conversaciones ficticias con estilo APX, en inglés y español. El pie visible, las leyendas y la procedencia aclaran que son maquetas y no conexiones reales. Las demás dejan capturas vacías hasta contar con una adecuada. No se usan datos de cuentas de trabajo.

[Contrato](INDEX.es.md): SHA-256 del ZIP, huella APX y procedencia. `DISTRIBUTION.json` registra las versiones listadas, `SHA256SUMS` cubre los archivos almacenados y `SOURCE-PROVENANCE.json` conserva el registro histórico inicial. Los archivos por hash ya publicados, incluso los no listados, se mantienen intactos.

Verificá los bytes antes de extraer. Descargar no autoriza habilitar: la instalación queda apagada. Revisá el mínimo `apx` de cada versión; el lote nuevo apunta a APX 2.16.0 y publicar el catálogo no actualiza ni libera APX. Leé README y skill: hay requisitos, cuentas, costos y límites. Inspeccionar o probar contratos no demuestra una cuenta real ni paridad completa. No se incluyen cuentas, claves ni créditos. Las credenciales van en el formulario/flujo aprobado, nunca en chat. Se mantienen las licencias externas y la atribución y licencia de imágenes documentadas para Open-Meteo.
