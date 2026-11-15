# agent-ready-plugins

Claude Code plugins for the Agent-Ready Directory. This repository holds only plugin files: no code, no secrets.

| Plugin | What it does |
| --- | --- |
| `agent-ready-directory` | Finds plumbers, HVAC companies and electricians near Austin, TX: the skill (how to handle a home-service request) and the connection to the directory's MCP server (`/mcp` on the API service, five tools) |

```
.claude-plugin/marketplace.json                       the catalogue: lists the plugins in this repository
plugins/agent-ready-directory/
  .claude-plugin/plugin.json                          name, version, the API key setting
  .mcp.json                                           the MCP server URL; the key is sent as a Bearer token
  skills/agent-ready-directory/SKILL.md               the skill
```

## Install (testers)

You need: Claude Code, read access to this repository, and an API key (`ard_...`) from the lead.

1. Let git reach this private repository without a prompt: `gh auth login`, then `gh auth setup-git`. Or use an
   SSH key that works without a passphrase prompt. Without a GitHub SSH key, set `CLAUDE_CODE_PLUGIN_PREFER_HTTPS=1`.
2. If you added the server by hand before (`claude mcp add agent-ready-directory ...`), remove it:
   `claude mcp remove agent-ready-directory`.
3. In a Claude Code session (not in the shell, which does not ask for the key):

```
/plugin marketplace add continuoustech-ai/agent-ready-plugins
/plugin install agent-ready-directory@agent-ready
```

4. Enter your API key when asked. Claude Code stores it in the system's secure storage, never in a file. To change it
   later: `/plugin configure agent-ready-directory@agent-ready`.
5. Turn on updates: `/plugin`, then **Marketplaces**, select `agent-ready`, **Enable auto-update**.
6. Check: `/mcp` lists `plugin:agent-ready-directory:agent-ready-directory` as connected. Then ask, for example:
   "My water heater is leaking, I'm in 78704".

Note: the `report_issue` tool writes a real report to the directory's database. When testing it, say it is a test.

## Change and release (maintainers)

Same flow as our other repositories: a branch, a pull request to `main`, the lead merges.

1. Edit the files (for the skill: `plugins/agent-ready-directory/skills/agent-ready-directory/SKILL.md`).
2. **Raise `version`** in `plugins/agent-ready-directory/.claude-plugin/plugin.json` (1.0.0 -> 1.1.0). Without a new
   version, users keep their old copy.
3. Check: `claude plugin validate .` from the repository root (the last line reads `Validation passed`).
4. Try it before the pull request: in a new folder, `/plugin marketplace add <path to this repository>`, install, ask
   a few questions.
5. Pull request; the lead merges.
6. Users get it through auto-update, or with `/plugin marketplace update agent-ready` and then `/reload-plugins`.

Also raise `version` when the server URL in `.mcp.json` or the key setting changes.
