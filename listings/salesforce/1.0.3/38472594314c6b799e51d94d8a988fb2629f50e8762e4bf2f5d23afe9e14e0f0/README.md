# Salesforce

Optional read-only adapter for accounts, contacts and opportunities in **one explicitly authorized Salesforce CLI org**. Two bounded tools, no arbitrary SOQL, writes, deployment, metadata execution or account scanning. Requires Node 24 and the supported `sf` CLI on **macOS or Linux**. Native Windows launching is not implemented; it fails before spawning a command.

## Why a CLI adapter

The official `@salesforce/mcp` local server [reached end of life on November 2, 2026](https://github.com/forcedotcom/mcp/issues/46); it is not installed or recommended. The replacement Salesforce skills are development workflows, not an APX CRM connector. This independent MIT wrapper reuses the supported CLI for OAuth, token refresh and vendor-managed credential storage instead of rebuilding those security-sensitive mechanisms. Node built-ins cannot replace that sign-in lifecycle safely. No CLI code is copied or bundled, and no npm package is downloaded automatically when the extension runs.

**External dependency:** Salesforce CLI `@salesforce/cli` (Apache-2.0), tested at **2.158.7** on Node 24/macOS ARM64. Install and maintain the CLI using Salesforce's official instructions; make `sf` available on the APX process's PATH. The extension does not pin or update an externally installed binary for you. CLI installation and org authorization are prerequisites, not something this package silently performs.

## Connect

1. Use a dedicated Salesforce user with API access and only the required **read** permissions on Account, Contact and Opportunity and their listed fields. Your edition must include API access. An administrator may need to permit the CLI OAuth client. The default CLI app can have broader OAuth authority: these read tools do not reduce its grant. For a custom client, an administrator should restrict it to the API and refresh-token scopes needed by the CLI; do not add full, metadata, deployment or write privileges to fix a failed read.
2. The owner signs in with the official CLI's browser flow, using a dedicated alias, for example `crm`. The owner completes browser credentials/SSO themselves. The CLI owns the resulting credential cache, refresh and revocation policy; no password or token is supplied to APX. See [web authorization](https://developer.salesforce.com/docs/atlas.en-us.sfdx_dev.meta/sfdx_dev/sfdx_dev_auth_web_flow.htm). Select the actual My Domain or sandbox when appropriate. Do not set a global default for this extension.
3. Inspect/install this folder. It installs **off**:

```bash
apx extensions inspect "$PWD/integrations/salesforce"
apx extensions install "$PWD/integrations/salesforce"
```

4. In Resources > Extensions > Salesforce, put that alias in `org_alias`, review the files and enable the package. The form uses an extension-scoped secret reference to pass a **configuration value, not an access token**. Only aliases with letters, digits, `_` and `-` are accepted, beginning with a letter; usernames, wildcards and dynamic-default sentinels are rejected. Every query includes `--target-org` explicitly. Do not repoint the alias at another account without reviewing the connection.

Do not run another CLI session for the same user during refresh. One read runs at a time per adapter process; parallel tool calls fail without launching another CLI. This cannot serialize other programs or separately installed copies. CLI/provider refresh-token rotation must remain enabled according to your organization's policy. If a grant expires, is revoked, or refresh is interrupted, the owner reauthorizes through the CLI rather than exposing tokens or weakening rotation. The wrapper never changes the keychain backend, exports credentials, calls `org display`, or edits CLI auth files. An inaccessible/locked keychain can cause a bounded failure; unlock it through the normal owner workflow, not by changing credential storage.

Disable/remove the package in APX when done. Revoke the client's access in Salesforce OAuth/connected-client settings and sign out the named CLI org if needed. Removing the package does not revoke the external CLI grant or delete its credential cache.

## Tools

- `salesforce_list`: `kind` accounts/contacts/opportunities, `limit` 1–20 (default 10), optional `exact_name` (literal equality, not wildcard/full-text search), optional observed `after_id`. It fetches at most `limit + 1` to determine `next_after_id`. This is keyset paging over current visible data, not a stable snapshot or total-account count.
- `salesforce_get`: the same kind plus an observed 15/18-character id. A null result means not visible/not found; do not infer deletion. Accounts expose Id/Name/Website/Industry; contacts Id/Name/FirstName/LastName/Title/AccountId; opportunities Id/Name/StageName/Amount/CloseDate/AccountId. No email/phone, custom/sensitive fields, associations, archived records or attachments. Do not infer an amount's currency.
- Only fixed `SELECT` statements with allowlisted objects and fields. Record ids and literal names are validated/escaped; no caller can choose a command, org, URL, field list or raw query. Arguments use `spawn` without a shell; the CLI runs in its own POSIX process group, which is terminated on cancellation, output overflow and parent exit so keychain helpers are not left behind; the working folder is the installed extension, not the active agent project. Provider attributes and CLI diagnostic envelopes are not returned. The external CLI and its installed plugins remain trusted dependencies with their own permissions; inspect and maintain them separately.
- [Request lifecycle / solicitudes](RUNTIME.md): 20-second total deadline, cancellation propagation, no automatic retries. CLI output is capped at 1 MiB, projected tool output is bounded and CRM text is marked untrusted. A cancelled subprocess is terminated; the provider may already have processed authentication or a read, so it is not an undo guarantee.

Salesforce receives the queries and selected private CRM data enters agent context. CLI/provider account and field permissions, API quotas, edition and subscription charges apply; no Salesforce subscription is included.

## Verification

Scripted CLI-contract tests and real child-process/stdio tests cover query generation, paging, explicit org pinning, malformed results, no-shell execution, sanitized failures and cancellation. An isolated official CLI 2.158.7 confirmed `NamedOrgNotFoundError` for an unauthenticated fixture alias, while the adapter returned only its safe local error. The fixture used an empty temporary home and its own generic test keychain; that setting is **not forced in production**. A native-keychain fixture initially waited past the deadline and left a helper after its parent exited. The owned helper was stopped. The runner now owns/terminates its POSIX process group, and a regression first reproduced an orphaned descendant's open socket before passing with full-group cancellation. None of this was treated as successful sign-in.

**UNVERIFIED:** live authorized queries, field/edition access, OAuth sign-in, token rotation/revocation and Linux execution. Windows is unsupported, not a passing test claim. The upstream OpenMausBot reference is a Composio service-name declaration, not vendored connector functionality. This is a read-only first increment, not full Salesforce API parity.

## Español

Extensión opcional de lectura para cuentas, contactos y oportunidades de **una organización autorizada expresamente en Salesforce CLI**. Requiere Node 24 y el comando `sf` en el PATH de APX, en macOS o Linux; Windows nativo todavía no está implementado. No instala el antiguo MCP oficial, que quedó fuera de soporte el 2 de noviembre de 2026, ni descarga dependencias al ejecutarse.

Usá un usuario dedicado con acceso a la API y solamente permisos de lectura sobre los objetos y campos necesarios. El administrador debe permitir el cliente OAuth correspondiente; el cliente predeterminado de la CLI puede tener permisos más amplios que estas herramientas. Iniciá sesión con el navegador oficial de la CLI y un alias dedicado, por ejemplo `crm`, sin cambiar la organización predeterminada. Instalá la extensión **apagada**, ingresá ese alias —no un token— en Recursos > Extensiones > Salesforce y revisá los archivos antes de habilitarla. No pegues credenciales en el chat.

La CLI administra la autenticación, la renovación y su almacenamiento de credenciales. La extensión no cambia el llavero ni exporta tokens. Si el llavero está bloqueado, desbloquealo por el procedimiento normal; no reduzcas las políticas de seguridad para hacer funcionar la prueba. No cambies el destino del alias sin revisar la conexión y evitá otras sesiones para el mismo usuario durante una renovación. Las herramientas se usan secuencialmente; las llamadas paralelas no lanzan otra CLI. Si se revoca o interrumpe la autorización, volvé a autenticarte sin desactivar la rotación exigida por tu organización.

Cada lista devuelve hasta 20 registros y un cursor por id, con filtro opcional de nombre exacto. No admite SOQL libre, edición, despliegues, correo, campos personalizados/sensibles, adjuntos ni datos archivados. Los textos son datos no confiables; un importe no implica una moneda. Tiene límite total de 20 segundos, cancelación, salida acotada y ningún reintento automático. Desinstalar no revoca la autorización externa: hacelo también en Salesforce y cerrá la sesión de la CLI cuando corresponda. Aplican el plan, las cuotas y los permisos de tu cuenta.

**Sin verificar:** cuenta real, consultas autorizadas, permisos de campos/edición, acceso por Linux, inicio de sesión OAuth y rotación/revocación. Se comprobó la negativa segura de la CLI oficial 2.158.7 con un perfil temporal sin cuenta; no se forzó otro backend de llavero en producción. El proceso y sus auxiliares se terminan juntos al cancelar; una regresión comprobó que no quede un descendiente con su conexión abierta. Es un primer alcance de lectura, no toda la API de Salesforce.

Docs impact: new README, bilingual listing, skill and capability matrix document the external CLI, named-org/read-only boundary, EOL notice and verification limits. No native APX permission, OAuth store or protocol changed.
