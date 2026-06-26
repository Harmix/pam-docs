> For Mintlify product knowledge (components, configuration, writing standards),
> install the Mintlify skill: `npx skills add https://mintlify.com/docs`

# PAM Developer Docs — agent instructions

## About this project

- Documentation site for **PAM Memory MCP** and the supporting **Developer REST API**
- Deployed at [manager.harmix.ai/docs](https://manager.harmix.ai/docs) (English default, Ukrainian in `uk/`)
- Pages are MDX files with YAML frontmatter
- Configuration lives in `docs.json`
- OpenAPI spec for Developer API: `openapi/developer-api.yaml`

## Terminology

| Term | Usage |
|------|-------|
| Memory MCP | The read-only MCP server at `POST /v1/mcp/memory` |
| `retrieve_memory` | The sole MCP tool in v1 |
| `pam_mkey` | Agent API key format: `pam_mkey_<key>` (desktop/script clients) |
| Browser OAuth | OAuth 2.1 + PKCE + DCR for Claude web and ChatGPT; scope `memory:read` |
| Connected apps | PAM UI + `GET/POST /v1/dev/oauth-connections*` for browser LLM sessions |
| Developer API | JWT-authenticated REST at `/v1/dev/*` |
| PAM app (Memory MCP UI) | Sidebar pages: `/setup`, `/access-management`, `/usage`, `/request-log`, `/playground` (legacy `/for-developers` redirects) |
| Readiness | Memory provisioning state before retrieval works |

## Navigation note

The **Developer API** tab in `docs.json` is temporarily hidden from Mintlify navigation. The MDX and OpenAPI files remain in the repo for re-enable later.

## Style preferences

- Use active voice and second person ("you")
- Keep sentences concise — one idea per sentence
- Use sentence case for headings
- Bold for UI elements: Click **Generate key**
- Code formatting for file names, commands, paths, and code references
- No marketing language ("powerful", "seamless", "robust")
- Realistic examples in code blocks — not foo/bar

## Content boundaries

**Document:**
- Memory MCP setup, protocol, `retrieve_memory`, quotas, errors
- Developer API endpoints for key management, readiness, usage, history, OAuth connections
- Browser MCP setup (OAuth discovery, DCR, refresh tokens, Connected apps)

**Do not document:**
- Internal admin features, architecture diagrams, or database table names
- PAM app sidebar / IA maps (no page-map tables listing Setup, Usage, Playground, etc.)
- General PAM Chat API
- Legacy paths (`/v1/memory/developer/*`) — use `/v1/dev/*`
- General PAM MCP at `/v1/mcp/router` (separate product surface)

## Source of truth

When updating docs, prefer live code over internal markdown:

| Topic | Source |
|-------|--------|
| MCP tool schema | `pam-agent-api/app/services/mcp/memory_tools.py` |
| Developer endpoints | `pam-agent-api/app/api/v1/developers/api.py` |
| Response models | `pam-agent-api/app/models/memory/mcp.py` |
| User-facing copy | `pam-frontend/locales/en/for-developers.json` |

## Client setup page (`client-setup.mdx`)

Keep connector guides in parity with the PAM app **Setup** page:

| App tabs | Docs location |
|----------|-----------------|
| Browser: ChatGPT, Claude, Perplexity, Other | `## Browser LLM connectors` → `<Tabs>` |
| Desktop: Cursor, Claude Code, Script | `## Desktop agents` → `<Tabs>` (+ VS Code in docs only) |

**Mintlify structure (required):**

- `<CardGroup>` jump links (Desktop vs Browser) at page top
- `<Steps>` inside each connector `<Tab>` (not numbered lists)
- `<AccordionGroup>` for advanced OAuth and manage-connections (collapsed by default)
- `<Expandable>` for script JSON-RPC example
- Inline links to [Setup](https://pam.harmix.ai/setup) / [Access](https://pam.harmix.ai/access-management) / [Request Log](https://pam.harmix.ai/request-log) — never a sidebar page-map table

Step copy: sync from `for-developers.json` when the app changes.

## Localization

- English pages live at the repo root (`introduction.mdx`, etc.)
- Ukrainian pages live in `uk/` with the same filenames
- Navigation uses `navigation.languages` in `docs.json` with `en` and `uk`
- Internal links in Ukrainian pages use the `/uk/` prefix (e.g. `/uk/quickstart`)
- OpenAPI reference (`openapi/developer-api.yaml`) is shared across languages

Before submitting doc changes:

```bash
mint broken-links
mint validate
```
