# Kooperativa plugin for Cursor

Find and qualify B2B leads from inside Cursor. This plugin connects Cursor to the Kooperativa data API through the official MCP server and adds skills and a rule that tell the agent how to use it.

Kooperativa returns structured people and company records: work history, education, skills, company details, headcount, decision makers, hiring signals and job changes. It has **no contact data**: no email addresses and no phone numbers.

## What is included

| Part | What it does |
|---|---|
| `mcp.json` | Starts `@kooperativa_team/mcp-server` (18 read-only tools) over stdio, pinned to an exact version. It sets only `KOOPERATIVA_MCP_TOOLSET=read` and contains no key |
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

## Example prompts

1. "We sell outbound analytics to US B2B software companies with 20 to 200 employees. Find 10 decision makers I can reach out to first." The skill confirms the ideal customer profile, searches companies, finds decision makers at the best matches, enriches only that shortlist, and reports caveats.
2. "List 15 companies in Germany in the logistics industry with 50 to 500 employees, and show which ones are hiring recently." Company search with the right filters, then hiring signals per company.
3. "Who are the VPs and directors at Example Analytics Inc., and which of them changed jobs in the last 90 days?" Decision makers for one account, then recent job changes.
4. "Find people similar to this profile and show their current employers." Lookalike search from a known person.

## Scope and safety

- All 18 tools are read-only lookups covered by the workspace license. This plugin does **not** include real-time (metered) lookups, webhook monitors, or email-to-profile resolution. The full 24-tool server is available separately on npm for people who want it.
- Profile and company text is written by third parties. The skills treat it as data and never as instructions.
- The skills are for sales and market research. They must not be used to screen, rank, or reject job candidates, to decide on credit, insurance, or housing, to compile dossiers on private individuals, or to track or harass anyone.
- A `451 DATA_SUBJECT_BLOCKED` response means a record was withdrawn at the person's request. The skills stop and do not try to retrieve it another way.

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
