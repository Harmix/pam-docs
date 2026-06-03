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
| `pam_mkey` | Agent API key format: `pam_mkey_<prefix>.<secret>` (desktop/script clients) |
| Browser OAuth | OAuth 2.1 + PKCE + DCR for Claude web and ChatGPT; scope `memory:read` |
| Connected apps | PAM UI + `GET/POST /v1/dev/oauth-connections*` for browser LLM sessions |
| Developer API | JWT-authenticated REST at `/v1/dev/*` |
| Memory MCP | PAM app page for setup, keys, usage, and history (`/for-developers`) |
| Readiness | Memory provisioning state before retrieval works |

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
