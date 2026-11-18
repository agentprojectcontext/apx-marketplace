# Home Assistant

Connect to your own Home Assistant instance to ask about the lights, sensors and automations it makes available. Start by finding out which devices and actions your server exposes, then use that context to request a specific state check or an intentional change. The available controls depend on your Home Assistant setup and the permissions you grant.

This connection uses your instance's address and a long-lived access token. It does not include a hosted Home Assistant service, hardware or a cloud subscription, and it does not grant access to every device automatically. Choose the entities you want to expose in Home Assistant, and check the reported state after requesting a change. If a device does not respond, check its availability in Home Assistant before trying again.

## Connection

Enter the base address of your instance (including any deployment path, but without the MCP endpoint suffix) as the Server setting. Store the access token in the Token secret. Enable MCP access in your instance first. The native endpoint suffix is /api/mcp; the extension preserves the HTTP transport and Bearer token header. No Node.js bridge is added.

Review the tools and account permissions before use. Live access has not been exercised. The configured URL was verified in an isolated APX core with a local HTTP MCP fixture: base addresses with zero, one or multiple trailing slashes and a deployment path reached the same native endpoint. This is connection-contract proof, not live Home Assistant access.

## Español

Conectá tu propia instancia de Home Assistant para consultar las luces, sensores y automatizaciones que exponga. Empezá por conocer qué dispositivos y acciones ofrece el servidor; después usá ese contexto para pedir una consulta de estado o un cambio concreto. Los controles disponibles dependen de tu configuración de Home Assistant y de los permisos que concedas.

La conexión usa la dirección de tu instancia y un token de acceso de larga duración. No incluye un servicio alojado de Home Assistant, dispositivos ni una suscripción a la nube, y no habilita automáticamente todos los equipos. Elegí las entidades que querés exponer desde Home Assistant y consultá el estado informado después de pedir un cambio. Si un dispositivo no responde, revisá su disponibilidad en Home Assistant antes de volver a intentarlo.

Ingresá la dirección base de tu instancia, incluida la ruta de despliegue si existe, pero sin el sufijo del endpoint MCP. Guardá el token como secreto y habilitá primero el acceso MCP. Revisá las herramientas y los permisos de la cuenta antes de usarla. No se verificó una cuenta real. Un core APX aislado verificó la conexión contra un servidor MCP HTTP de prueba: con cero, una o varias barras finales y una ruta de despliegue se alcanzó el mismo endpoint nativo. Esto prueba el contrato de conexión, no el acceso real a Home Assistant.
