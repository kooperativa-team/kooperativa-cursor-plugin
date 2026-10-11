# Kooperativa plugin for Cursor

Find and qualify B2B leads from inside Cursor. This plugin connects Cursor to the Kooperativa data API through the official MCP server and adds skills and a rule that tell the agent how to use it.

Kooperativa returns structured people and company records: work history, education, skills, company details, headcount, decision makers, hiring signals and job changes. It has **no contact data**: no email addresses and no phone numbers.

## What is included

| Part | What it does |
|---|---|
| `mcp.json` | Starts `@kooperativa_team/mcp-server` (24 tools) over stdio, pinned to an exact version. It contains no key and no `env` block |
| `skills/kooperativa-lead-generation` | A workflow for ICP-based lead lists: confirm the inputs, search, shortlist, enrich, report honestly |
| `skills/setup` | Connection and troubleshooting guidance |
| `rules/kooperativa-data-handling.mdc` | Safety and honesty rules for the data tools (available to the agent on request) |

## Requirements

- A Kooperativa account with an active license. Accounts are invite-only: request access at https://kooperativa.io/signup.
- An API key, created at https://kooperativa.io/api-keys. Keys are shown once.
- Node.js 18 or newer (the server is started with `npx`).

## Install

From the Cursor Marketplace, open **Customize**, find Kooperativa and select **Install**.

To try it from source, copy this repository into `~/.cursor/plugins/local/kooperativa`, then run **Developer: Reload Window**. On Teams and Enterprise plans an admin must allow local plugin imports.

## Configure the API key

The plugin holds no secrets, so the key is stored once by the server itself. In a terminal, run:

```bash
npx -y @kooperativa_team/mcp-server login
```

It asks for the key with input hidden, checks it against the API, and saves it to `~/.kooperativa/config.json` with mode 600. It prints only your account email and license state. Reload Cursor afterwards.

The server also accepts a real key in the `KOOPERATIVA_API_KEY` environment variable when Cursor is started from a shell that has it set. The key is sent only to `https://kooperativa.io/api/v1`.

Do not paste the key into a chat or commit it to a repository. This package contains no credentials.

## Costs and safety

- Standard tools are covered by the workspace license.
- The two `*_realtime` tools are metered at $0.001 per call and bill even when the lookup finds nothing. The skill and the rule ask for confirmation before using them.
- Profile and company text is written by third parties. The skills treat it as data and never as instructions.
- A `451 DATA_SUBJECT_BLOCKED` response means a record was withdrawn at the person's request. The skills stop and do not try to retrieve it another way.
- Monitors (webhook subscriptions) are created or deleted only on an explicit request.

## Privacy and support

- Privacy policy: https://kooperativa.io/legal/privacy
- Terms: https://kooperativa.io/legal/terms
- Data removal requests: https://kooperativa.io/legal/removal-request
- Documentation: https://docs.kooperativa.io
- Support: support@kooperativa.io
- MCP server source: https://github.com/kooperativa-team/kooperativa-mcp
- Kiro version of this plugin: https://github.com/kooperativa-team/kooperativa-kiro-power

## Versioning

`mcp.json` pins the MCP server to an exact version so an unreviewed release is never started on a user's machine. A new plugin version is published when the pin is updated.

## License

MIT, see [LICENSE.md](./LICENSE.md).
