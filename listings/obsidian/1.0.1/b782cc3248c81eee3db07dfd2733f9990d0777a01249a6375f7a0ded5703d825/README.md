# Obsidian

This official port retains the native command, `npx -y mcp-obsidian`, with the selected vault as its single argument. It uses the current manifest 2 plain-setting contract, `setting://vault`; it does not put the folder in the secret store or replace the upstream server with another adapter.

## Connect

Install the extension off, review it, then choose an existing vault folder in its **Vault folder** setting before enabling it. The computer running the core needs Node.js, npm and access to that folder. APX validates the directory setting. The upstream process has the computer's normal permissions, not a separate filesystem sandbox. Inspect its actual tools before requesting writes.

No Obsidian account, Sync or Publish subscription is configured by this package. The note text that an agent reads can enter its conversation and model context. Keep credentials out of the folder setting and use a demonstration vault for examples, never private work notes.

## Verification

The manifest is structurally checked against the native command and the current APX typed-setting schema. A runtime configuration check is recorded in the task evidence separately; manifest inspection does not prove that the current npm package can perform every note operation. No private vault has been read or changed for this listing. The listing intentionally omits screenshots until suitable imagery is ready.

## Español

El paquete conserva `npx -y mcp-obsidian` con la carpeta elegida como argumento. Usa el ajuste común `setting://vault` del manifest 2 actual: no guarda la carpeta como contraseña ni reemplaza el servidor por otro adaptador.

Instalá la extensión apagada, revisala, elegí una carpeta de vault existente y después activala. La computadora del core necesita Node.js, npm y acceso a esa carpeta. Rigen los permisos normales del proceso, sin aislamiento extra del sistema de archivos. Revisá las herramientas reales antes de pedir escrituras. No se conecta una cuenta de Obsidian Sync ni Publish. El texto de notas leído puede entrar al chat y su proveedor de modelos.

No se leyeron ni cambiaron notas privadas para preparar esta ficha. Se verifican por separado la equivalencia de la definición, la configuración en APX y las operaciones reales del servidor. La lista de capturas queda vacía hasta tener una imagen adecuada.

Docs impact: updated — connection, local permissions, data sharing and verification boundaries are documented here and in the bilingual listing. No APX source changes.
