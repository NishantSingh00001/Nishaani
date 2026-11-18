# Security Review — Nishaani

Nishaani is a fully static, client-side oral-history stewardship workbench
(`index.html` + `app.js`, 7 hash-routed pages). Story records, transcripts,
rights labels, exhibitions and activity are stored only in browser
`localStorage`. Confirmed no `fetch`/`XMLHttpRequest`/`WebSocket` call
anywhere in `app.js`/`index.html` — nothing leaves the browser; an attached
audio recording is held only as an in-memory object URL
(`URL.createObjectURL`/`URL.revokeObjectURL`) for same-tab preview and is
never uploaded or included in JSON backups, consistent with the README.

Note: like several of this author's other repos, `Nishaani-Vercel.zip` is a
zipped duplicate of the tracked source files, checked into the repo. Not a
security issue, but redundant — worth removing or `.gitignore`-ing. Not
changed in this review.

## 20-point pen-test checklist

| # | Check | Applicable? | Notes |
|---|-------|-------------|-------|
| 1 | HTTP/2 Rapid Reset (CVE-2023-44487) | N/A | No server component in this repo. |
| 2 | bcrypt 72-byte truncation | N/A | No password hashing. |
| 3 | SAML signature bypass (CVE-2024-45409) | N/A | No SAML/SSO. |
| 4 | nOAuth | N/A | No OAuth/auth of any kind. |
| 5 | GraphQL alias batching | N/A | No GraphQL. |
| 6 | TOTP replay (RFC 6238 §5.2) | N/A | No TOTP/2FA. |
| 7 | ReDoS | No | No user-supplied-pattern regexes; the local word-frequency "index suggestions" feature uses fixed splitting/stop-word logic, not a vulnerable regex. |
| 8 | JWT algorithm confusion | N/A | No JWTs. |
| 9 | HTTP request smuggling | N/A | No server. |
| 10 | Web cache poisoning | N/A | No server/cache layer. |
| 11 | Spoofed X-Forwarded-For | N/A | No server-side request handling. |
| 12 | Postgres views bypassing RLS | N/A | No database. |
| 13 | `Math.random()` for security tokens | No | `Math.random()` is used once (`createStory()`, `app.js:126`) to generate a local record ID (`'n-'+Math.random().toString(36)...`) — a display/reference ID, not a security token or access credential. |
| 14 | Timing attacks on secret comparison | N/A | No secrets/credentials to compare. |
| 15 | Padding oracle | N/A | No encryption/decryption. |
| 16 | Server-side prototype pollution | N/A | No server; `localStorage` state is loaded into a fixed shape, not deep-merged from attacker input. |
| 17 | Server-side template injection | N/A | No server, no templating engine. |
| 18 | XXE | N/A | No XML parsing (JSON export/import only). |
| 19 | Cross-site WebSocket hijacking | N/A | No WebSockets. |
| 20 | Unicode username normalization | N/A | No usernames/accounts. |

## Findings outside the 20-point list

None. Every dynamic `innerHTML` write that includes user-entered content —
story title/speaker/place/language/excerpt/transcript/source in
`storyDetail()`, exhibition title/description in the exhibition-detail and
`previewExhibit()` panels, and the catalogue/transcript list views — passes
that text through `esc()` (`app.js:15`, escaping `&`/`<`/`>`/`"`/`'`) before
interpolation, including the free-text transcript body (only a literal
`\n`→`<br>` replacement is applied after escaping, which is safe). The
50 MB file-size cap on `attachAudio()` is enforced before creating the object
URL.

No fixes are proposed or applied in this review.
