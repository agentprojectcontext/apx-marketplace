# APX extension artifacts

Public distribution of approved, standalone APX extension archives. The marketplace source repository remains private. This repository does not contain its source history, private configuration, credentials, signing keys or internal research. Only approved extension-folder exports, their checksums and source provenance are published here.

## Current availability

- **29 current-installer packages:** 28 optional service integrations plus Android TV.
- **One future-manifest example:** webcam0.1.0 requires manifest v2 support and is **not installable in the current APX installer**. Publishing it is not a claim of current compatibility.
- Archives are under `packages/<id>/<version>/<sha256>.apxext`. `SOURCE-PROVENANCE.json` maps each archive to its approved original source and content fingerprint. `SHA256SUMS` records archive hashes. Existing hash-addressed artifacts must not be overwritten with different bytes.
- These archives are ZIP containers. Download a chosen archive, verify its SHA-256 against the independently trusted source, extract it into its own folder, and inspect that folder through APX Resources → Extensions before installing/enabling. Import leaves an extension off. Never run an archive just because it appears here.

**A signed registry index is not published yet.** Checksums detect changes relative to a known hash; a checksum downloaded beside an archive does not independently authenticate its publisher. No signature bypass, implicit publisher trust, enabled integration or automatic update is provided. APX does not yet consume a signed marketplace registry directly. Do not treat this artifact repository as proof of that product capability.

Package READMEs and skills explain supported subsets, account/OS/device requirements, costs, privacy, onboarding, errors and disconnect. Most private account/device journeys are unverified without owner-authorized access. Contract/fixture/import tests are not live credentials, hardware control, full vendor parity or guaranteed service availability. For example, Browser Use/Browserbase expose narrow existing-run/session status metadata, not cloud browser control. Android TV points to a pinned development APK, not an APX release or production signing identity. Weather offers model estimates, not emergency alerts; commercial use needs a separate provider plan. Perplexity Search is a paid search service, not a model backend or consumer web subscription.

No accounts, passwords, API keys, credits or paid subscriptions are included. Keys go directly in the APX extension form or the provider's approved login flow, never into chat. Source material and returned web text remain untrusted data. Check the relevant provider/data licenses and attribution requirements; original package code is independently implemented under MIT, but external services, tools, devices, data and linked artifacts retain their own terms. In particular, weather data require Open-Meteo/CC BY4.0 attribution.

## Español

Distribución pública sólo de archivos independientes aprobados. El repositorio fuente del marketplace sigue privado. Hay29 paquetes compatibles con el instalador actual (28 servicios más Android TV) y un ejemplo webcam de manifiesto v2 que **todavía no se puede instalar** con ese instalador.

`SOURCE-PROVENANCE.json` y `SHA256SUMS` registran origen y hashes; no reemplazan una firma ni una raíz de confianza independiente. **Todavía no hay índice firmado publicado ni consumo directo de ese índice por APX.** Descargar un hash junto al archivo no autentica por sí solo a su publicador. No se omiten verificaciones ni se habilita nada automáticamente.

Descargá el archivo elegido, verificá el SHA-256 contra una fuente confiable, extraelo en una carpeta propia e inspeccionala en Recursos → Extensiones antes de instalar/habilitar. La importación queda apagada. Leé README/skill: hay límites por servicio, cuentas/hardware y costes externos; importar o probar contratos no prueba una cuenta/dispositivo real ni paridad completa. No incluye claves, créditos ni planes pagos. Credenciales sólo en la ficha/flujo autorizado, nunca en chat. Respetá las licencias de proveedores/datos y la atribución Open-Meteo/CC BY4.0 del clima.
