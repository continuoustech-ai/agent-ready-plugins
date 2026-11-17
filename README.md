# agent-ready-plugins

Plugins for the Agent-Ready Directory, for Claude Code (Claude), Antigravity (Gemini) and Codex (GPT). This repository
holds only plugin files: no code, no secrets.

**Website:** https://mcp.myownline.com (what it is, the docs at `/docs`, your API key at `/keys`).

| Plugin | What it does |
| --- | --- |
| `agent-ready-directory` | Finds plumbers, HVAC companies and electricians near Austin, TX: the skill (how to handle a home-service request) and the connection to the directory's MCP server (`/mcp` on the API service, five tools) |

```
.claude-plugin/marketplace.json                       the catalogue: lists the plugins in this repository (Claude Code, Codex)
plugins/agent-ready-directory/
  .claude-plugin/plugin.json                          Claude Code: name, version, the API key setting
  .mcp.json                                           Claude Code: the MCP server URL; the key is sent as a Bearer token
  .antigravity-plugin/plugin.json                     Antigravity: name, version, suggested prompts
  mcp_config.json                                     Antigravity: the MCP server URL; the key (ARD_API_KEY) is sent as X-API-Key
  plugin.json                                         Codex: name, version, listing details (Agent Plugins format)
  mcp.json                                            Codex: the MCP server URL; declares an API key sent as a Bearer token
  skills/agent-ready-directory/SKILL.md               the skill (all three)
```

All three apps use the same skill and the same server; only the manifest and the way the key is set up differ. Each
app reads only its own files: Antigravity ignores `plugin.json` and `mcp.json`, and Codex uses them instead of
Claude's `.mcp.json`.

## Install in Claude Code

You need: Claude Code and an API key (`ard_...`).

1. **Get a key (free):** open https://mcp.myownline.com/keys, sign in with your email (we send a link), accept the terms and create a
   key. It is shown **once**: copy it. Up to 3 keys per account; 30 calls a minute and 1,000 a day for the account.
   Lost it? Create a new one there and revoke the old one.
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

### Troubleshooting (Claude Code)

- **Moved to https://mcp.myownline.com (1.3.0):** the old run.app address still works, but update the plugin:
  `/plugin marketplace update agent-ready`, then update `agent-ready-directory` in `/plugin` and restart. Your key
  stays the same. A server you added by hand (`claude mcp add ...`): remove it and use the plugin.
- **"Dynamic Client Registration rejected (HTTP 404)"** after Authenticate or Reconnect: the server got no key or a
  wrong one. It uses API keys, not sign-in: do not use Authenticate. Set the key with
  `/plugin configure agent-ready-directory@agent-ready`, then restart Claude Code.
- **Reconnect does not ask for the key.** It uses the saved one; only install and `/plugin configure` ask.
- **The tools show twice:** you also have a hand-made server; `claude mcp remove agent-ready-directory`.
- **Lost the key:** it cannot be shown again. Make a new one at https://mcp.myownline.com/keys, revoke the old one, set the new one.
- **`daily_limit`:** the account used today's calls (30 a minute, 1,000 a day); they reset at 00:00 UTC.

## Install in Antigravity (Gemini)

You need: the Antigravity CLI (`agy`) and an API key (`ard_...`) from https://mcp.myownline.com/keys.

1. Install the CLI if you do not have it (Windows PowerShell: `irm https://antigravity.google/cli/install.ps1 | iex`;
   other systems: Antigravity's Getting Started page). Open a new terminal and check: `agy --version`.
2. Get this repository and install the plugin from its folder:

```
git clone https://github.com/continuoustech-ai/agent-ready-plugins.git
agy plugin install agent-ready-plugins/plugins/agent-ready-directory
```

   Do not use `agy plugin import claude` for this plugin: it drops the key setting and every call fails with 401.
3. The plugin installs switched off until the key is set. Run `agy`, type `/plugin`, open **Installed**, select
   `agent-ready-directory`, press **Space** and enter your API key. (Desktop app: Customizations, then Plugins.)
4. Check: `agy plugin list` shows `agent-ready-directory` as enabled. Pick a Gemini model, then ask, for example:
   "My water heater is leaking, I'm in 78704". Antigravity may ask you to approve each tool call until you allow the
   tools.
5. Updates: there is no auto-update from this repository. `git pull` in your copy, then
   `agy plugin uninstall agent-ready-directory` and install again (step 2); you may be asked for the key again.

## Install in Codex (GPT)

You need: Node.js 18 or later, a ChatGPT account (or an OpenAI API key), and an API key (`ard_...`) from
https://mcp.myownline.com/keys.

In the Codex CLI the plugin gives the skill, but it cannot ask for the API key, so you add the server once yourself
(step 4). The plugin's own server entry stays unused, so the tools do not show twice.

1. Install Codex and sign in: `npm install -g @openai/codex`, then `codex login`.
   Windows PowerShell: if `codex` fails with "running scripts is disabled on this system", use `codex.cmd` instead of
   `codex` everywhere below, or allow local scripts once: `Set-ExecutionPolicy -Scope CurrentUser RemoteSigned`.
2. Add this catalogue and install the plugin:

```
codex plugin marketplace add continuoustech-ai/agent-ready-plugins
codex plugin add agent-ready-directory@agent-ready
```

3. Save your key in the environment variable `ARD_API_KEY`. Windows: `setx ARD_API_KEY "ard_..."`, then close every
   VS Code window and terminal and open a new one. macOS or Linux: add `export ARD_API_KEY="ard_..."` to `~/.zshrc`
   or `~/.bashrc` and open a new terminal.
4. Add the server (once), in a terminal that has the key:

```
codex mcp add agent-ready-directory --url https://mcp.myownline.com/mcp --bearer-token-env-var ARD_API_KEY
```

5. Check: start `codex`, type `/mcp`: `agent-ready-directory` shows as connected (5 tools). Then ask, for example:
   "My water heater is leaking, I'm in 78704".
6. Updates: `codex plugin marketplace upgrade agent-ready`, then `codex plugin add agent-ready-directory@agent-ready`.

### Troubleshooting (Codex)

- **`/mcp` shows `agent-ready-directory: failed`; F2 shows "Environment variable ARD_API_KEY ... is not set":**
  Codex keeps a background service running between sessions, and it started before your key was set. In a terminal
  where the key is set (`if ($env:ARD_API_KEY) { "ok" }` in PowerShell), quit Codex, run
  `codex app-server daemon stop`, then start `codex` again.
- **The key is set but a VS Code terminal does not see it:** VS Code terminals copy the environment VS Code started
  with. Close every VS Code window and open it again, or load it into this terminal (PowerShell):
  `$env:ARD_API_KEY = [Environment]::GetEnvironmentVariable("ARD_API_KEY", "User")`.
- **`bad_key`:** the key is mistyped or revoked; make a new one at
  https://mcp.myownline.com/keys and set it again (step 3).
- **Uninstall:** `codex plugin remove agent-ready-directory@agent-ready`, `codex plugin marketplace remove agent-ready`,
  `codex mcp remove agent-ready-directory`.

Note (all apps): the `report_issue` tool writes a real report to the directory's database. When testing it, say it is
a test.

## Change and release (maintainers)

Same flow as our other repositories: a branch, a pull request to `main`, the lead merges.

1. Edit the files (for the skill: `plugins/agent-ready-directory/skills/agent-ready-directory/SKILL.md`).
2. **Raise `version`** in all three manifests, `.claude-plugin/plugin.json`, `.antigravity-plugin/plugin.json` and
   `plugin.json` (1.3.0 -> 1.4.0). Without a new version, Claude Code and Codex users keep their old copy.
3. Check from the repository root:
   - `claude plugin validate .` (the last line reads `Validation passed`)
   - `agy plugin validate plugins/agent-ready-directory` (passes with three expected warnings: Antigravity ignores
     Claude's `.mcp.json` and Codex's `plugin.json` and `mcp.json`)
4. Try it before the pull request: in Claude Code, in a new folder, `/plugin marketplace add <path to this
   repository>`, install, ask a few questions; in Antigravity, `agy plugin install <path>/plugins/agent-ready-directory`,
   turn it on, ask the same questions; in Codex, `codex plugin marketplace add <path to this repository>`,
   `codex plugin add agent-ready-directory@agent-ready`, ask the same questions.
5. Pull request; the lead merges.
6. Claude Code users get it through auto-update, or with `/plugin marketplace update agent-ready` and then
   `/reload-plugins`. Antigravity users pull and reinstall (see above). Codex users run
   `codex plugin marketplace upgrade agent-ready` and install again.

Also raise `version` when the server URL or the key setting changes, and change it in `.mcp.json`, `mcp_config.json`
and `mcp.json` (and in the Codex install command above).
