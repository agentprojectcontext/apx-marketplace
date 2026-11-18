# Canvas LMS

Version 1.0.1: [request cancellation and deadline / cancelación y límite de solicitudes](RUNTIME.md). Reinstall to update an installed copy; approval is not inherited automatically.

Optional APX extension for listing your courses and their assignments/due dates. Independently implemented with Node built-ins; no Python, external CLI or npm package is required. This is not affiliated with Instructure.

## Connect

1. In your Canvas cloud school account, open Account > Settings > Approved integrations and create a short-lived personal access token. Your school may disable this; ask its administrator rather than bypassing the policy.
2. Install this folder with `apx extensions inspect <folder>` then `apx extensions install <folder>`. Installation is off. In Resources > Extensions > Canvas LMS enter `api_token` and `instance_url`, for example `https://school.instructure.com`. The address is stored in the extension's own secret store because the current extension manifest has no separate non-secret settings field. Never put a real token in a file, command argument or chat.
3. Enable it yourself in that screen. Ask for your courses, then assignments in a selected course. Empty results can mean no visible courses; use the appropriate enrollment state if needed.

The two tools are read-only. A personal token may have broader permissions inherited from its user: use a minimally privileged account and short expiry. Where an institution issues scoped developer-key credentials, only GET courses and GET course assignments are needed. This package does not register an OAuth client or implement an institution's OAuth consent flow; personal tokens have no automatic refresh.

## Boundaries and recovery

- HTTPS single-label `*.instructure.com` school origins only. Custom domains, self-hosted servers and local networks are not supported in this release; no token is sent to another host via redirects or pagination.
- One page of 1–20 results per call; use `next_page`, not the provider's raw Link URL. Each request has a 20-second deadline, 1 MiB response cap and no automatic retry. Outputs are bounded and token echoes redacted.
- Authentication/permission errors: check the school account and token expiry. Revoke the token in Canvas Account > Settings > Approved integrations, create a replacement and update the extension secret. Turning the extension off alone does not revoke the vendor token.
- Rate limit: wait and try later. Network errors are sanitized. No grades, submissions, rosters or writes are exposed.
- Requires an existing Canvas account and the institution's access/plan. This package has no additional cloud subscription; Canvas account availability and rate limits remain the institution's responsibility.

## Verification and provenance

Contract tests exercise pagination, argument validation, destination containment, redaction, errors and a real stdio process. A live school/token check is still **UNVERIFIED**, not inferred from tests. Source capabilities were identified in the Hermes Canvas skill; no upstream implementation was copied. Source SHA/license references are in `docs/reference-integrations/services.json`. Official API: https://developerdocs.instructure.com/services/canvas/resources/courses and https://developerdocs.instructure.com/services/canvas/resources/assignments.

Docs impact: added setup, least privilege, expiry/revocation, costs, supported-host limits and verification status in English and Spanish.
