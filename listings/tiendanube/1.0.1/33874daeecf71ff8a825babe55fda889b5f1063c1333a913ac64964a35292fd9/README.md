# Tiendanube

APX's connector for the remote MCP server operated by Tiendanube at `https://admin-mcp.tiendanube.com/`. The listing and package are published by APX; no commercial endorsement by Tiendanube is implied.

## Connect your own store

1. Install the extension from the APX Marketplace. It installs off.
2. Review the connection, then enable it when you are ready to authorize your own account.
3. When the MCP bridge starts, follow the browser sign-in flow on the computer running APX. Review the store and permissions before accepting. Cancel if they are not what you intended.
4. After authorization, inspect the actual tools available to your account. Review and confirm store changes before executing them. This connection is not read-only.

Requires APX 2.16.1 or newer, Node.js 20.18.1 or newer and npm. Earlier APX versions are not supported because they do not isolate OAuth caches for this pinned bridge. The package runs `npx -y mcp-remote@0.17.1 https://admin-mcp.tiendanube.com/`. The bridge version is pinned, but npm still resolves its transitive dependencies at installation; this is not a vendored or fully locked dependency tree. Internet access to npm and Tiendanube is required. A remote/headless host may need a browser and access to the bridge's local OAuth callback on that host.

OAuth credentials belong to the person signing in, not the package publisher. Compatible APX versions inject a dedicated `MCP_REMOTE_CONFIG_DIR` scoped to this APX installation, extension and server, separate from manual bridges and other extensions. The bridge's `~/.mcp-auth` default applies to manual use outside that isolation, not this extension in compatible APX. Do not distribute this directory or reuse another person's cached authorization. For a separate account or test, use an isolated OS profile or authentication directory and authorize that account yourself. Disabling the extension stops its connection; revoke account access through Tiendanube when you no longer need it.

## Verification boundary

The endpoint challenges unauthenticated MCP initialization with HTTP 401 and advertises OAuth authorization, token and registration endpoints with S256 PKCE. Its advertised scopes include writes; scopes are not proof of any particular MCP tool. Packaging and APX installation can be checked without a store account. Full login, tool discovery after login and real store operations remain unverified without an approved test account. Do not infer tool capabilities or read-only access from this listing.

## Español

Conector de APX para el servidor MCP remoto operado por Tiendanube en `https://admin-mcp.tiendanube.com/`. APX publica la ficha y el paquete; no se implica un aval comercial de Tiendanube.

1. Instalá la extensión desde el Marketplace de APX. Se instala apagada.
2. Revisá la conexión y activala cuando estés listo para autorizar tu propia cuenta.
3. Al arrancar el puente MCP, seguí el inicio de sesión en el navegador de la computadora que ejecuta APX. Revisá la tienda y los permisos antes de aceptar. Cancelá si no corresponden.
4. Después de autorizar, revisá las herramientas realmente disponibles para tu cuenta. Revisá y confirmá los cambios antes de ejecutarlos. No es una conexión de solo lectura.

Requiere APX 2.16.1 o posterior, Node.js 20.18.1 o posterior y npm. Las versiones anteriores de APX no separan la caché OAuth de este puente con versión fijada y no están soportadas. Ejecuta `npx -y mcp-remote@0.17.1 https://admin-mcp.tiendanube.com/`. La versión del puente está fijada, pero npm resuelve sus dependencias transitivas al instalar; no es una distribución con todas las dependencias congeladas. Requiere acceso a npm y Tiendanube. En un equipo remoto o sin interfaz puede hacer falta un navegador y acceso al retorno OAuth local de ese equipo.

Las credenciales OAuth pertenecen a quien inicia sesión, no al publicador. Las versiones compatibles de APX inyectan un `MCP_REMOTE_CONFIG_DIR` propio de esta instalación de APX, extensión y servidor, separado de puentes manuales y otras extensiones. El valor por defecto `~/.mcp-auth` corresponde al uso manual fuera de ese aislamiento, no a esta extensión en APX compatible. No compartas esa carpeta ni reutilices la autorización de otra persona. Para otra cuenta o una prueba, usá un perfil del sistema o directorio de autenticación aislado y autorizá esa cuenta. Desactivar la extensión detiene su conexión; revocá el acceso desde Tiendanube cuando ya no lo necesites.

El servidor responde HTTP 401 a la inicialización MCP sin credenciales y anuncia autorización OAuth, token, registro y PKCE S256. Anuncia permisos de escritura; eso no demuestra la existencia de herramientas concretas. El empaquetado y la instalación en APX se pueden comprobar sin cuenta. Sin una cuenta de prueba aprobada, quedan sin verificar el inicio de sesión completo, las herramientas disponibles tras autorizar y las operaciones reales sobre tiendas.

Docs impact: updated — new bilingual setup, credential isolation, dependency pinning, write confirmation and account-verification limits. No APX runtime changes.
