# AI Content Drop plugins for Claude

A plugin marketplace for Claude Code, claude.ai and Claude Desktop, published by AI Content Drop, the AI video studio. It lists two plugins:

| Plugin | What it is | Install id |
| --- | --- | --- |
| [AI Content Drop Studio](./plugins/aicontentdrop-studio/README.md) | The connector (the MCP server at `https://aicontentdrop.com/mcp`), eight slash commands that drive its tools with every paid step quoted and approved first, and the agent skills. Generates media through your own account. | `aicontentdrop-studio@aicontentdrop` |
| [AI Content Drop](https://github.com/aicontentdrop/claude-plugin) | The advisory plugin, built for Anthropic's plugin directory: pick the model, write the prompt, price the job, outline the episode. Text only; no connector, no sign-in. | `aicontentdrop@aicontentdrop` |

## Add the marketplace

Claude Code, inside a session or from the shell:

```text
/plugin marketplace add aicontentdrop/claude-plugins
/plugin install aicontentdrop-studio@aicontentdrop
```

```bash
claude plugin marketplace add aicontentdrop/claude-plugins
claude plugin install aicontentdrop-studio@aicontentdrop
```

claude.ai and Claude Desktop: **Customize > Plugins > Add > Add marketplace**, enter `aicontentdrop/claude-plugins`, then install the plugin you want. For the studio plugin, open its **Connectors** tab afterwards, add **AI Content Drop** and sign in.

## Layout

```text
.claude-plugin/marketplace.json      the catalogue
plugins/aicontentdrop-studio/        the studio plugin, byte for byte as released
```

The studio plugin's files are generated from the [`aicontentdrop`](https://github.com/aicontentdrop/aicontentdrop) monorepo by `scripts/build-claude-plugin-studio.mjs`, and a test there fails the build if this copy drifts from the source. The advisory plugin lives in its own repository and is fetched from it at install time.

MIT licensed. Support: <support@aicontentdrop.com>. Plan questions go to <https://aicontentdrop.com/plans>.
