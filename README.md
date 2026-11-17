# APX Marketplace

Public distribution of APX's approved standalone extension archives and official catalog metadata. The source workshop is `apx-marketplace-private` and stays private. No private repository history, research, configuration, credentials or signing keys are published here.

## Official catalog

The owner-approved initial catalog lists **30 compatible packages**, including Android TV. The official endpoint is `https://agentprojectcontext.github.io/apx-marketplace/index.json`. The future manifest v2 webcam example remains unlisted. The schema 1 index is intentionally unsigned; no signing identity is required or created. See the [index contract](INDEX.md) ([Español](INDEX.es.md)) for archive SHA-256, APX file fingerprints, forward compatibility and provenance rules. There is no third-party submission service or public announcement.

Archives outside the curated index stay in this repository but are not official catalog entries. Existing hash-addressed bytes must not be overwritten. `SHA256SUMS` records exact archive hashes; `SOURCE-PROVENANCE.json` records approved package commits. The source repository rename changes its location, not the package contents or hashes.

## Using an archive

Archives are ZIP containers under `packages/<id>/<version>/<sha256>.apxext`. Verify the exact bytes against the trusted index before safe extraction. Installation leaves an extension off; downloading does not authorize enabling code. APX archive installation and index consumption are separate implementation phases, not capabilities established by this repository alone.

Read the package README and skill for supported subsets, account/OS/device requirements, costs, privacy, onboarding and limitations. Most live account/device journeys remain unverified. Contract tests and imports are not full vendor parity or live-service verification. No accounts, API keys, credits or paid plans are included. Enter credentials only in APX's approved form/login flow, never in chat. External service, data and device licenses still apply, including Open-Meteo/CC BY 4.0 attribution for weather data. The webcam manifest v2 example is not compatible with the current v1 installer; Android TV uses a pinned development APK.

## Español

Distribución pública de archivos independientes aprobados y metadatos del catálogo oficial. El taller `apx-marketplace-private` sigue privado; no se publican historial, investigación, configuración ni claves privados.

El catálogo inicial aprobado lista **30 paquetes compatibles**, incluido Android TV. El endpoint oficial es `https://agentprojectcontext.github.io/apx-marketplace/index.json`, schema 1 por HTTPS y deliberadamente sin firma. El ejemplo futuro de webcam con manifiesto v2 queda sin listar. [Contrato](INDEX.es.md): SHA-256 del ZIP, huella APX, compatibilidad y procedencia. No hay publicación de terceros ni anuncio público. Los archivos no listados quedan en el repo, fuera del catálogo oficial; los bytes con nombre por hash son inmutables.

Verificá los bytes contra el índice confiable antes de extraer. Descargar no autoriza habilitar: la instalación queda apagada. Leé README y skill de cada paquete; hay requisitos de sistema, cuentas, costos y límites. Las pruebas de contrato/importación no prueban cuentas reales ni paridad completa. Las claves van en el formulario/flujo aprobado, nunca en chat. Se mantienen las licencias externas y la atribución Open-Meteo/CC BY 4.0. Webcam v2 no es compatible con el instalador v1; Android TV usa un APK de desarrollo.
