# WooCommerce

Use WooCommerce 11.3 or later on WordPress 6.9 or later, or the higher WordPress minimum your installed versions require. Activate the official [WordPress MCP Adapter](https://developer.woocommerce.com/docs/features/mcp/) and keep its default endpoint available. The old WooCommerce-specific endpoint is not supported.

1. Create a dedicated WordPress user with only the permissions it needs. Do not use an administrator's account.
2. In that user's WordPress profile, create an application password for APX.
3. In WooCommerce's connection form in APX, enter the store's full HTTPS address (for example, `https://shop.example.com`), the WordPress username and the application password. Include the installation subfolder if WordPress uses one. Paste the application password as WordPress gives it to you; do not use your sign-in password.
4. Connect and review the available tools first. APX can make changes if your site and the user's permissions allow them. Data returned from the store may enter the chat context.

If you connected using the old Base64 setup, revoke the application password you used for it in WordPress. Reconnect with the full HTTPS address and a new application password. Updating clears the old local credential. Going back also requires reconnecting; APX does not restore the old password. Don't reuse the old password if you go back to an earlier version.

To change stores, revoke the old application password in WordPress first. Create one on the new site, then enter the new address and application password together in WooCommerce settings in APX. In APX, you can set the WooCommerce tools to **Ask**, especially those that can make changes. A request in chat is not an approval setting.

## Español

Usá WooCommerce 11.3 o posterior con WordPress 6.9 o posterior, o la versión mínima de WordPress que requiera tu instalación si es más alta. Activá el [adaptador MCP oficial de WordPress](https://developer.woocommerce.com/docs/features/mcp/) y dejá disponible su dirección predeterminada. La dirección anterior, específica de WooCommerce, no es compatible.

1. Creá un usuario dedicado de WordPress con solo los permisos que necesite. No uses una cuenta de administrador.
2. En el perfil de ese usuario en WordPress, creá una contraseña de aplicación para APX.
3. En el formulario de conexión de WooCommerce en APX, ingresá la dirección completa de la tienda con HTTPS (por ejemplo, `https://shop.example.com`), el usuario de WordPress y la contraseña de aplicación. Incluí la subcarpeta de instalación si WordPress usa una. Pegá la contraseña de aplicación tal como te la da WordPress; no uses tu contraseña de inicio de sesión.
4. Conectá y revisá primero las herramientas disponibles. APX puede hacer cambios si tu sitio y los permisos del usuario lo permiten. Los datos que devuelva la tienda pueden entrar en el contexto del chat.

Si te conectaste con la configuración anterior en Base64, revocá en WordPress la contraseña de aplicación que usaste. Conectá de nuevo con la dirección completa con HTTPS y una contraseña de aplicación nueva. La actualización elimina la credencial local anterior. Volver atrás también exige reconectar: APX no restaura la contraseña vieja. Si volvés a una versión anterior, no reutilices la contraseña vieja.

Para cambiar de tienda, revocá primero la contraseña de aplicación anterior en WordPress. Creá una en el nuevo sitio e ingresá la nueva dirección y la contraseña juntas en la configuración de WooCommerce en APX. En APX, podés configurar las herramientas de WooCommerce para que **Pregunten**, sobre todo las que pueden hacer cambios. Pedir confirmación en el chat no reemplaza ese ajuste.
