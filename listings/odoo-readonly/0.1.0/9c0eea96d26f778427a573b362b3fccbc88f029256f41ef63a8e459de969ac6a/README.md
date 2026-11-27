# Odoo Community 19 (read-only)

An APX-built connector for **self-hosted Odoo Community 19 with JSON-2 enabled**. Requires APX 2.17.2 or newer and a Node 24 runtime available as `node`. It connects one HTTPS origin, database and user identity. Not an official Odoo MCP server. No Odoo Online, Odoo.sh, Enterprise or Custom deployment has been verified.

## Before connecting

1. Install the Odoo Community 19 applications you need: Contacts, CRM for opportunities, and Sales for quotations/orders. Confirm that your deployment exposes JSON-2.
2. Create a dedicated **internal user**, not a portal account, with only the required read access, company access and record rules. A read-only adapter does not make the API key read-only outside this adapter.
3. Sign in as that user. In account preferences, open **Security → API Keys → Add API Key**, confirm your identity if prompted and choose a short expiry. Replace the key before it expires and revoke any key you no longer use. Do not use a sign-in password in APX.
4. Enter the exact HTTPS origin (for example `https://odoo.example.com`, without a path), database name and API key in APX. The key is stored as an extension secret. Verify the destination before saving it.
5. Enable the connection and ask APX to read a small page of contacts. If access fails, verify the key expiry, database, HTTPS certificate and Odoo user permissions. An empty page may mean that no records match the user's record rules.

Use a normally trusted HTTPS certificate in production. The disposable verification fixture uses a private test CA trusted only by its MCP child; this is not a requirement to disable TLS verification or modify Odoo. No additional Odoo module or JSON-2 code patch is needed by the tested Community 19 fixture.

## What it reads

The only tool is `odoo_list_records`. Choose `contacts` (ID, name, company flag), `opportunities` (ID, name, type, active flag) or `sales` (ID, reference, state, date/time). CRM excludes leads. Sales includes quotations and orders. Contacts include all partners visible to the dedicated identity, not just customers.

Pages default to 10 rows, with a maximum of 20, in ascending ID order. Pass `nextAfterId` as `afterId` for the same collection. Pages are not a snapshot. Names are capped at 256 characters; stored text is untrusted and returned records may enter chat context.

The adapter cannot write, accept arbitrary models/methods/fields/domains, change companies or switch identity through a tool argument. Odoo permissions and record rules still apply. Modified models and fields, other Odoo versions and commercial deployments are not supported by this candidate. There are no redirects, retries or legacy RPC fallback. This is not a process-level network sandbox: configure only an origin you control.

## Español

Conector de APX de **solo lectura para Odoo Community 19 autohospedado, con JSON-2 habilitado**. Requiere APX 2.17.2 o posterior y Node 24 disponible como `node`. Una dirección HTTPS, una base y una identidad por conexión. No es un servidor MCP oficial de Odoo. No se verificaron Odoo Online, Odoo.sh, Enterprise ni Custom.

Instalá Contactos y, si los necesitás, CRM y Ventas. Creá un **usuario interno dedicado**, no una cuenta de portal, con el acceso mínimo, las reglas de registros y las empresas adecuadas. Iniciá sesión con esa cuenta y, en sus preferencias, entrá a **Seguridad → Claves API → Agregar clave API**. Si te lo pide, confirmá tu identidad y elegí un vencimiento corto. Reemplazá la clave antes de que venza y revocá las que no uses. En APX, ingresá la clave API (no tu contraseña de inicio de sesión), la dirección HTTPS sin ruta y el nombre exacto de la base. Verificá el destino y activá la conexión. No hace falta modificar Odoo ni desactivar la validación del certificado HTTPS.

Consultá contactos, oportunidades (no leads) y presupuestos o pedidos de venta, con los campos seleccionados que permite la cuenta. Cada página tiene hasta 20 registros; la siguiente no es una instantánea de la anterior. Un resultado vacío puede deberse a las reglas de acceso. Los registros devueltos pueden entrar en el contexto del chat. No modifica datos ni admite modelos, métodos o campos arbitrarios; la clave puede tener permisos adicionales fuera del conector. No se admiten versiones anteriores, modelos modificados ni despliegues comerciales no probados.

Si falla, revisá vencimiento de la clave, nombre de la base, certificado HTTPS y permisos del usuario. Este candidato no tiene costo de conector; el alojamiento de Odoo se contrata aparte.
