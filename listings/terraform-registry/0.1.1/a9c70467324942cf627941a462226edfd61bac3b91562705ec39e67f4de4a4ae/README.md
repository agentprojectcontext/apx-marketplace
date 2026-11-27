# Terraform Registry

Look up public Terraform modules and providers, and check which versions the registry publishes. This is the Registry-only option, not the general Terraform integration: it does not use a Terraform account or manage workspaces, runs or infrastructure.

1. Start Docker on the computer running APX, and make sure it can reach the public Terraform Registry.
2. Open Terraform Registry in Marketplace and choose **Connect**. There is no sign-in or key to enter.
3. Ask: “What version of `terraform-aws-modules/vpc/aws` is published in the Terraform Registry?”

If the connection does not start, check that Docker is running and can reach the image registry, then retry. If a lookup fails, check that APX can reach [the public Registry](https://registry.terraform.io/) and that the module address is public. Don't include private names or credentials in lookups.

Runtime: [HashiCorp's local Terraform MCP server](https://developer.hashicorp.com/terraform/mcp-server/deploy/local), pinned in `apx-extension.json`, with only the Registry toolset, no Terraform credentials and no host mounts.

## Español

Buscá módulos y proveedores públicos de Terraform y consultá las versiones que ofrece el registro. Esta ficha es solo para el registro público, no es la integración general de Terraform: no usa una cuenta ni administra espacios de trabajo, ejecuciones o infraestructura.

1. Abrí Docker en el equipo donde corre APX y comprobá que tenga acceso al Registro público de Terraform.
2. Abrí Terraform Registry en Marketplace y elegí **Conectar**. No tenés que iniciar sesión ni ingresar una clave.
3. Preguntá: «¿Qué versión de `terraform-aws-modules/vpc/aws` está publicada en el Registro de Terraform?».

Si la conexión no arranca, fijate si Docker está abierto y puede acceder al registro de imágenes. Después volvé a intentar. Si falla una consulta, comprobá que APX llegue al [registro público](https://registry.terraform.io/) y que la dirección del módulo sea pública. No incluyas nombres privados ni claves en las búsquedas.

Entorno de ejecución: [servidor local de Terraform de HashiCorp](https://developer.hashicorp.com/terraform/mcp-server/deploy/local), con la versión fijada en `apx-extension.json`, solo herramientas del registro, sin credenciales de Terraform ni archivos montados del equipo.
