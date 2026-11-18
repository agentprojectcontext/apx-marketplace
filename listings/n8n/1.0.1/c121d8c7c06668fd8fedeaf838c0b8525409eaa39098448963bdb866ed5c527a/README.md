# n8n

Bring the workflows of your own n8n instance into a conversation. Discover the workflows you have made available, review their purpose and inputs, and choose which one you want to use. Execution and discovery depend on the tools exposed by your instance; installing this extension does not create a workflow or connect its underlying services for you.

Use the address of your instance and its MCP access token. A workflow may contact other services or change their data when it runs, so review the selected workflow and its inputs rather than treating every run as a read-only query. n8n hosting, licensing, cloud plans and the accounts or usage charged by a workflow's services remain separate. No finished workflow, account credential or execution credit is included.

## Connection

Enter the base address of your instance (including any deployment path, but without the MCP endpoint suffix) as the Server setting. Store the access token in the Token secret. Enable MCP access in your instance first. The native endpoint suffix is /mcp-server/http; the extension preserves the HTTP transport and Bearer token header. No Node.js bridge is added.

Review the tools and account permissions before use. Live access has not been exercised. The configured URL was verified in an isolated APX core with a local HTTP MCP fixture: base addresses with zero, one or multiple trailing slashes and a deployment path reached the same native endpoint. This is connection-contract proof, not live n8n access.

## Español

Llevá los workflows de tu propia instancia de n8n a la conversación. Descubrí cuáles pusiste a disposición, revisá su propósito y sus entradas, y elegí el que querés usar. La búsqueda y la ejecución dependen de las herramientas que exponga tu instancia: instalar esta extensión no crea un workflow ni conecta por vos sus servicios externos.

Usá la dirección de tu instancia y su token de acceso MCP. Un workflow puede comunicarse con otros servicios o modificar sus datos cuando se ejecuta; revisá el elegido y sus entradas, en vez de considerar cada ejecución una consulta de lectura. El alojamiento, la licencia y los planes de n8n, además de las cuentas y consumos de los servicios usados por cada workflow, se gestionan por separado. No se incluyen workflows terminados, credenciales ni créditos de ejecución.

Ingresá la dirección base de tu instancia, incluida la ruta de despliegue si existe, pero sin el sufijo del endpoint MCP. Guardá el token como secreto y habilitá primero el acceso MCP. Revisá las herramientas y los permisos de la cuenta antes de usarla. No se verificó una cuenta real. Un core APX aislado verificó la conexión contra un servidor MCP HTTP de prueba: con cero, una o varias barras finales y una ruta de despliegue se alcanzó el mismo endpoint nativo. Esto prueba el contrato de conexión, no el acceso real a n8n.
