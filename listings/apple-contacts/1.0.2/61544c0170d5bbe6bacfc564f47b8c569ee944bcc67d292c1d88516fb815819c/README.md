# Apple Contacts

Independent dependency-free MCP adapter for **one uniquely named Contacts group on macOS**. `apple_contacts_list` returns a page of names/IDs; `apple_contacts_get` reads the selected member's name, email addresses and phones. Initial read-only subset: no all-address-book search, edits, private notes, photos, dates, postal addresses, vCards, account sync, messages or APX contact import/linking. This is not the iCloud web API or the full Apple bundle.

## Setup

- Node 24 and macOS Contacts must be available on the host running the MCP process. A paired computer does not redirect a remote core's process. Other operating systems fail before starting a script.
- Choose a small Contacts group/list with a **unique exact name**. Enter that name in `group_name` in **Resources > Extensions > Apple Contacts**. The form value is stored by APX and injected through `secret://group_name`; it is scope configuration, not an account credential. The tools cannot override it. Duplicate/absent names fail; there is no broad fallback. The adapter examines up to 256 group references/names but reads person fields only through the selected group.
- Review the folder, then inspect/install **off**:

```bash
apx extensions inspect "$PWD/integrations/apple-contacts"
apx extensions install "$PWD/integrations/apple-contacts"
```

- The owner chooses whether to enable it and grant macOS Automation/privacy access for Contacts to the actual execution host/application. First use may launch Contacts or prompt for permission. The agent must stop for that decision, not click through prompts, edit privacy databases, read private address-book files or request Full Disk Access as a workaround. No permission or personal account is accessed during installation off.

The OS permission can expose the entire address book; this adapter's group boundary is not a provider/OS-issued group-only grant. A contact can belong to several groups; its emails/phones are not group-specific. The same record may change after it is selected. A name/email/phone does not verify identity or authorization to contact someone. Use APX's existing contact/channel workflows for any later message, with their approvals. No sending fallback exists here.

## Behavior and recovery

Default page size 10, maximum 20; explicit offset 0–2000. Selected group maximum 2,000 person references. There is no full-book search or background scan. Names/IDs only are read on a list; get accepts an observed ID in the selected group and returns at most 20 emails and 20 phones. Membership, group identity/name and person ID are checked around reads; concurrent changes reject rather than deliberately broadening scope. These checks are not an atomic OS snapshot and pagination can shift.

Request data goes as JSON on stdin to a fixed JXA script, never interpolated into source or a shell command. `osascript` runs with no shell, a minimal inherited environment, 20-second child deadline and 1 MiB combined stdout/stderr limit. It is cancelled/cleaned up as a process group on macOS, including descendants. Raw diagnostics are suppressed and no automatic retry occurs. Results are untrusted, string values above 6,000 characters are explicitly truncated. Large groups or contacts with excessive fields fail rather than silently returning a complete-looking subset. Group reads can be slow; the deadline fails instead of continuing indefinitely.

If a group disappears/is ambiguous, a member moves, permissions are denied or scripting fails, review the exact selection and system privacy settings directly. Do not broaden the group, bypass privacy or use raw database files. Missing members return `record: null`, not identity verification. Disabling/removing the extension stops its server/skill, but **does not revoke macOS permissions**; the owner manages those in System Settings. No OAuth, refresh, network login, new package, routine, telemetry or paid API is used.

## Provenance and verification

Independent MIT implementation/instructions. Reference declaration: Openbot `f3e3f8ade9c146e96d9d68d65770d2a9126a7856`, `marketplace/plugin-catalog/plugins/apple-apps/plugin.json`, PolyForm-Noncommercial-1.0.0. No code or prompts from that package are copied. The installed macOS Contacts scripting dictionary was consulted for public group/person/email/phone properties; private account data was not read. The standalone bounded subprocess runner is adapted from this repository's own MIT Apple Notes adapter, not the incompatible external bundle or unbounded third-party helper.

Tests run the same CommonJS/JXA function with an injected application model: strict inputs, selected group isolation, paging, detail field limits and membership changes. Actual subprocess/stdio paths and a real JXA invalid-input probe are exercised without constructing Contacts. **UNVERIFIED:** successful authorized JXA against Contacts, actual OS prompts/grant revocation, nested/smart group behavior, linked-record semantics, provider sync, all supported macOS versions and large real address books. Fixture success or source installation is not that proof. Future edits/search remain explicit gaps.

Docs impact: English/Spanish setup/card, agent skill, service status and verification evidence. No APX native/protocol/release/UI change or public registry index claim.

## Español

Adaptador MCP independiente y sin dependencias para **un grupo de Contactos de macOS con nombre exacto y único**. Lista nombres/IDs por páginas y lee nombre, correos y teléfonos de un miembro elegido. Es un subconjunto inicial: no busca en toda la agenda, modifica registros, lee notas privadas/fotos/fechas/direcciones postales/vCards, sincroniza, manda mensajes ni importa/vincula contactos de APX. No es una API web de iCloud ni paridad con el paquete Apple completo.

Requiere Node 24, macOS y Contactos donde se ejecuta el proceso MCP. Emparejar otra computadora no redirige un core remoto. Elegí un grupo/lista pequeño y de nombre único; ingresá su nombre en `group_name` de **Recursos > Extensiones > Apple Contacts**. Se guarda como configuración de alcance mediante la referencia propia `secret://group_name`, no como credencial de cuenta. El agente no puede cambiarlo desde los argumentos. Un nombre ausente o duplicado falla sin ampliar el alcance. Se revisan hasta 256 referencias/nombres de grupos, sin leer campos personales de los otros grupos.

Inspeccioná e instalá apagada con los comandos anteriores. El dueño revisa antes de habilitarla y decide los permisos de automatización/privacidad de macOS para el equipo/app que realmente ejecuta el proceso. El primer uso puede abrir Contactos o pedir permiso: el agente se detiene y no acepta por vos, toca bases de privacidad, lee archivos privados ni pide acceso total al disco como atajo. Instalar apagada no abre una cuenta personal ni concede permisos.

El permiso del sistema puede ser amplio: el filtro de grupo lo aplica este adaptador, no un permiso exclusivo para ese grupo. Un contacto puede pertenecer a varios grupos y sus correos/teléfonos son comunes. Los datos pueden cambiar; encontrar un teléfono no demuestra identidad ni autoriza a escribirle. Para mensajes posteriores se mantienen los contactos, canales y aprobaciones de APX.

Páginas de 10 por defecto, máximo 20 y offset explícito hasta 2000. Grupo máximo de 2000 personas; detalles con hasta 20 correos y 20 teléfonos. Se comprueban ID, grupo y pertenencia antes/después, sin prometer una foto atómica; la paginación puede cambiar. JSON entra por stdin a un script fijo sin shell ni interpolación. El proceso tiene plazo de 20 segundos, salida combinada máxima de 1 MiB, cancelación/limpieza de descendientes y ningún reintento. No se muestran diagnósticos privados. Se marca el recorte de textos mayores a 6000 caracteres; grupos o campos excesivos fallan, no simulan cobertura completa.

Ante rechazo, cambio de grupo o falta de permisos, revisá la selección y los ajustes de macOS, sin ampliar acceso o leer la base de datos. Deshabilitar/eliminar la extensión no revoca permisos del sistema: el dueño lo hace en Ajustes del Sistema. No hay OAuth, renovación, login de red, analítica ni consumo pago. Implementación MIT propia, sin copiar código o instrucciones del referente PolyForm no comercial.

Se prueban la misma lógica con app inyectada, subprocesos/stdio reales y JXA con entrada inválida antes de construir Contactos. **Sin verificar:** lectura autorizada real, permisos/revocación, grupos anidados/inteligentes, contactos vinculados, sincronización, versiones de macOS y agendas grandes. No se leyó la agenda personal ni se pidió permiso del sistema. Las pruebas simuladas no prueban esos recorridos ni la futura edición/búsqueda.
