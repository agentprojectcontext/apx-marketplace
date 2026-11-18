# Official index contract

Endpoint: `https://marketplace.apxagent.app/index.json`.

`index.json` uses **schema 1**, is served over HTTPS and is intentionally **unsigned**. Only APX-approved entries are listed; third-party publishing is not supported. Unlisted archives are not part of the official catalog. Trust is anchored in this official HTTPS endpoint, not a signature or a checksum downloaded from an arbitrary mirror.

Consumers read each entry's `id`, `kind`, `name`, `description`, `categories`, `tier`, and each version's:

- `version`: semantic version.
- `apx`: minimum APX version.
- `content_hash`: 16 lowercase hexadecimal characters, the APX file fingerprint.
- `archive.url`: credential-free HTTPS URL of the standalone `.apxext` ZIP in this repository.
- `archive.sha256`: 64 lowercase hexadecimal characters, SHA-256 of the exact downloaded archive bytes (no `sha256-` prefix).

Current rich entries also expose:

- `tagline`: a short English description, with translated copy in the listing.
- `category`: exactly one of `productivity`, `communication`, `docs-notes`, `files`, `development`, `data`, `design-media`, `sales-customers`, `finance`, `marketing`, `devices-home`, `search-research`, `business-systems`. This singular field is distinct from the older broad `categories` array.
- `listing`: absolute HTTPS URL of the versioned `apx-listing.json`.
- `icon`: absolute HTTPS URL of its 256 px SVG or PNG.

Listing schema 2 carries bilingual long descriptions, three prompts per language, connection details, links, publisher and screenshot captions. `screenshots: []` is permitted when no suitable image is ready. Explicitly labeled fictional APX-styled conversations are illustrations, never live-account proof; they must not imitate the service's own interface. Listing schema and executable manifest version are independent.

Additional schema 1 fields may include publisher, card, source and integrity metadata. Consumers ignore unknown fields. If `schema` is greater than supported, ignore the whole index and request an APX update. Validate required known fields before use.

Verify the downloaded archive SHA-256 before extraction. After safe extraction, calculate APX's file fingerprint: sort relative file paths with JavaScript's default string sort; for each file hash its UTF-8 path, a NUL byte, its raw bytes, then another NUL byte into one SHA-256 stream. `content_hash` is the first 16 hexadecimal characters of that digest. Paths are relative with `/` separators. This differs from ZIP SHA-256: recompression can change `archive.sha256` without changing `content_hash`.

Official provenance matches **id + version + content_hash** from this index. A matching archive hash alone does not grant that status. Downloading does not authorize enabling an extension; APX's installation and approval boundaries still apply. This contract does not assert that every released APX version already implements index consumption or archive installation.

Existing hash-addressed archives remain immutable. Source provenance records identify original package commits, whose repository was renamed to `apx-marketplace-private`; private repository history is not published here.

[Español](INDEX.es.md)
