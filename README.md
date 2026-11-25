# agent-ready-plugins

Plugins for Kanvi (by AgentsAI), for Claude Code (Claude), Antigravity (Gemini) and Codex (GPT). This repository
holds only plugin files: no code, no secrets.

**Website:** https://api.kanvi.agentsai.in (what it is, the docs at `/docs`, your API key at `/keys`).

| Plugin | What it does |
| --- | --- |
| `kanvi` | Finds plumbers, HVAC companies and electricians in Texas: the skill (how to handle a request) and the connection to the directory's MCP server (`/mcp` on the API service, five tools) |

```
.claude-plugin/marketplace.json                       the catalogue: lists the plugins in this repository (Claude Code, Codex)
plugins/kanvi/
  .claude-plugin/plugin.json                          Claude Code: name, version, the API key setting
  .mcp.json                                           Claude Code: the MCP server URL; the key is sent as a Bearer token
  .antigravity-plugin/plugin.json                     Antigravity: name, version, suggested prompts
  mcp_config.json                                     Antigravity: the MCP server URL; the key (ARD_API_KEY) is sent as X-API-Key
  plugin.json                                         Codex: name, version, listing details (Agent Plugins format)
  mcp.json                                            Codex: the MCP server URL; declares an API key sent as a Bearer token
  skills/kanvi/SKILL.md                               the skill (all three apps): the rules for every trade
  skills/kanvi/home-services.md                       the skill: plumbing, HVAC, electrical (read when needed)
  skills/kanvi/visit-businesses.md                    the skill: salons and barbers (read when needed)
```

All three apps use the same skill and the same server; only the manifest and the way the key is set up differ. Each
app reads only its own files: Antigravity ignores `plugin.json` and `mcp.json`, and Codex uses them instead of
Claude's `.mcp.json`.

## Install in Claude Code

You need: Claude Code and an API key (`ard_...`).

1. **Get a key (free):** open https://api.kanvi.agentsai.in/keys, sign in with your email (we send a link), accept the terms and create a
   key. It is shown **once**: copy it. Up to 3 keys per account; 30 calls a minute and 1,000 a day for the account.
   Lost it? Create a new one there and revoke the old one.
2. If you added the server by hand before (`claude mcp add agent-ready-directory ...`, `... agents-ai ...` or
   `... kanvi ...`), remove it: `claude mcp remove agent-ready-directory` (or `agents-ai`, or `kanvi`).
3. In a Claude Code session (not in the shell, which does not ask for the key):

```
/plugin marketplace add continuoustech-ai/agent-ready-plugins
/plugin install kanvi@agents-ai
```

4. Enter your API key when asked. Claude Code stores it in the system's secure storage, never in a file. To change it
   later: `/plugin configure kanvi@agents-ai`.
5. Turn on updates: `/plugin`, then **Marketplaces**, select `agents-ai`, **Enable auto-update**.
6. Check: `/mcp` lists `plugin:kanvi:kanvi` as connected. Then ask, for example:
   "My water heater is leaking, I'm in 78704".

### Troubleshooting (Claude Code)

- **1.6.1:** the skill describes the new result order (in the place, then nearby, then businesses serving the area
  from farther away). Auto-update gets it; otherwise `/plugin marketplace update agents-ai`.
- **New address (1.6.0):** the server is now `https://api.kanvi.agentsai.in/mcp`. With auto-update on, nothing to do;
  otherwise `/plugin marketplace update agents-ai`, then restart Claude Code. The old address keeps working, and the
  same key works on both.
- **Renamed to Kanvi (1.5.0):** the plugin was `agent-ready-directory` from the marketplace `agent-ready`. Remove the old one and install the new one:
  `/plugin uninstall agent-ready-directory@agent-ready`, `/plugin marketplace remove agent-ready`, then the two commands
  in step 3. You enter the key again (the same key works). A server you added by hand: `claude mcp remove
  agent-ready-directory`. Installed a pre-release `agents-ai@agents-ai`? `/plugin uninstall agents-ai@agents-ai`
  first.
- **"Dynamic Client Registration rejected (HTTP 404)"** after Authenticate or Reconnect: the server got no key or a
  wrong one. It uses API keys, not sign-in: do not use Authenticate. Set the key with
  `/plugin configure kanvi@agents-ai`, then restart Claude Code.
- **Reconnect does not ask for the key.** It uses the saved one; only install and `/plugin configure` ask.
- **The tools show twice:** you also have a hand-made server; `claude mcp remove kanvi`.
- **Lost the key:** it cannot be shown again. Make a new one at https://api.kanvi.agentsai.in/keys, revoke the old one, set the new one.
- **`daily_limit`:** the account used today's calls (30 a minute, 1,000 a day); they reset at 00:00 UTC.

## Install in Antigravity (Gemini)

You need: the Antigravity CLI (`agy`) and an API key (`ard_...`) from https://api.kanvi.agentsai.in/keys.

1. Install the CLI if you do not have it (Windows PowerShell: `irm https://antigravity.google/cli/install.ps1 | iex`;
   other systems: Antigravity's Getting Started page). Open a new terminal and check: `agy --version`.
2. Get this repository and install the plugin from its folder:

```
git clone https://github.com/continuoustech-ai/agent-ready-plugins.git
agy plugin install agent-ready-plugins/plugins/kanvi
```

   Do not use `agy plugin import claude` for this plugin: it drops the key setting and every call fails with 401.
3. The plugin installs switched off until the key is set. Run `agy`, type `/plugin`, open **Installed**, select
   `kanvi`, press **Space** and enter your API key. (Desktop app: Customizations, then Plugins.)
4. Check: `agy plugin list` shows `kanvi` as enabled. Pick a Gemini model, then ask, for example:
   "My water heater is leaking, I'm in 78704". Antigravity may ask you to approve each tool call until you allow the
   tools.
5. Updates: there is no auto-update from this repository. `git pull` in your copy, then
   `agy plugin uninstall kanvi` and install again (step 2); you may be asked for the key again.
6. New address (1.6.0): the server is now `https://api.kanvi.agentsai.in/mcp`. `git pull`, then uninstall and
   install again (step 5). The old address keeps working.
7. Renamed to Kanvi (1.5.0): the old plugin was `agent-ready-directory` (or `agents-ai` in a pre-release). After
   `git pull`, run `agy plugin uninstall agent-ready-directory` (or `agents-ai`), then install the new folder
   (step 2) and set the key again (step 3).

## Install in Codex (GPT)

You need: Node.js 18 or later, a ChatGPT account (or an OpenAI API key), and an API key (`ard_...`) from
https://api.kanvi.agentsai.in/keys.

In the Codex CLI the plugin gives the skill, but it cannot ask for the API key, so you add the server once yourself
(step 4). The plugin's own server entry stays unused, so the tools do not show twice.

1. Install Codex and sign in: `npm install -g @openai/codex`, then `codex login`.
   Windows PowerShell: if `codex` fails with "running scripts is disabled on this system", use `codex.cmd` instead of
   `codex` everywhere below, or allow local scripts once: `Set-ExecutionPolicy -Scope CurrentUser RemoteSigned`.
2. Add this catalogue and install the plugin:

```
codex plugin marketplace add continuoustech-ai/agent-ready-plugins
codex plugin add kanvi@agents-ai
```

3. Save your key in the environment variable `ARD_API_KEY`. Windows: `setx ARD_API_KEY "ard_..."`, then close every
   VS Code window and terminal and open a new one. macOS or Linux: add `export ARD_API_KEY="ard_..."` to `~/.zshrc`
   or `~/.bashrc` and open a new terminal.
4. Add the server (once), in a terminal that has the key:

```
codex mcp add kanvi --url https://api.kanvi.agentsai.in/mcp --bearer-token-env-var ARD_API_KEY
```

5. Check: start `codex`, type `/mcp`: `kanvi` shows as connected (5 tools). Then ask, for example:
   "My water heater is leaking, I'm in 78704".
6. Updates: `codex plugin marketplace upgrade agents-ai`, then `codex plugin add kanvi@agents-ai`.
7. New address (1.6.0): the server is now `https://api.kanvi.agentsai.in/mcp`. A server added by hand before keeps
   working on the old address; to move it, `codex mcp remove kanvi`, then step 4 again.
8. Renamed to Kanvi (1.5.0): the old names were `agent-ready-directory` (plugin and server) and `agent-ready`
   (catalogue). Remove them: `codex plugin remove agent-ready-directory@agent-ready`,
   `codex plugin marketplace remove agent-ready`, `codex mcp remove agent-ready-directory`; then steps 2 and 4.
   The key in `ARD_API_KEY` stays. A pre-release installed as `agents-ai@agents-ai`: also
   `codex plugin remove agents-ai@agents-ai` and `codex mcp remove agents-ai`.

### Troubleshooting (Codex)

- **`/mcp` shows `kanvi: failed`; F2 shows "Environment variable ARD_API_KEY ... is not set":**
  Codex keeps a background service running between sessions, and it started before your key was set. In a terminal
  where the key is set (`if ($env:ARD_API_KEY) { "ok" }` in PowerShell), quit Codex, run
  `codex app-server daemon stop`, then start `codex` again.
- **The key is set but a VS Code terminal does not see it:** VS Code terminals copy the environment VS Code started
  with. Close every VS Code window and open it again, or load it into this terminal (PowerShell):
  `$env:ARD_API_KEY = [Environment]::GetEnvironmentVariable("ARD_API_KEY", "User")`.
- **`bad_key`:** the key is mistyped or revoked; make a new one at
  https://api.kanvi.agentsai.in/keys and set it again (step 3).
- **Uninstall:** `codex plugin remove kanvi@agents-ai`, `codex plugin marketplace remove agents-ai`,
  `codex mcp remove kanvi`.

Note (all apps): the `report_issue` tool writes a real report to the directory's database. When testing it, say it is
a test.

## Change and release (maintainers)

Same flow as our other repositories: a branch, a pull request to `main`, the lead merges.

1. Edit the files. The skill's source is the directory repository (`agent/skill/kanvi/`: `SKILL.md`,
   `home-services.md`, `visit-businesses.md`); copy all three into `plugins/kanvi/skills/kanvi/`
   unchanged, never edit them only here.
2. **Raise `version`** in all three manifests, `.claude-plugin/plugin.json`, `.antigravity-plugin/plugin.json` and
   `plugin.json` (for example 1.5.0 -> 1.6.0). Without a new version, Claude Code and Codex users keep their old copy.
3. Check from the repository root:
   - `claude plugin validate .` (the last line reads `Validation passed`)
   - `agy plugin validate plugins/kanvi` (passes with three expected warnings: Antigravity ignores
     Claude's `.mcp.json` and Codex's `plugin.json` and `mcp.json`)
4. Try it before the pull request: in Claude Code, in a new folder, `/plugin marketplace add <path to this
   repository>`, install, ask a few questions; in Antigravity, `agy plugin install <path>/plugins/kanvi`,
   turn it on, ask the same questions; in Codex, `codex plugin marketplace add <path to this repository>`,
   `codex plugin add kanvi@agents-ai`, ask the same questions.
5. Pull request; the lead merges.
6. Claude Code users get it through auto-update, or with `/plugin marketplace update agents-ai` and then
   `/reload-plugins`. Antigravity users pull and reinstall (see above). Codex users run
   `codex plugin marketplace upgrade agents-ai` and install again.

Also raise `version` when the server URL or the key setting changes, and change it in `.mcp.json`, `mcp_config.json`
and `mcp.json` (and in the Codex install command above).

## Official MCP Registry (maintainers)

`server.json` lists the server in the official MCP Registry (https://registry.modelcontextprotocol.io) as
`in.agentsai/kanvi`: only the name, a description and the live URL (`https://api.kanvi.agentsai.in/mcp`), no code.
Aggregators such as Glama, PulseMCP and GitHub's MCP list read it from there. It does not list us in Claude, ChatGPT
or Gemini; those take their own submissions.

- **Name:** `in.agentsai` is our domain `agentsai.in` reversed. The registry lets only the owner of that domain
  publish under it, proven by a DNS TXT record on `agentsai.in`. The name cannot be changed once published.
- **Key:** an Ed25519 key pair. The private key is kept outside every repository (the lead's machine and the team's
  secret store, never committed); its public half is the TXT record
  `v=MCPv1; k=ed25519; p=<public key>` on `agentsai.in` (the root, not a subdomain). Leave that record in place.
- **Publish or update** (raise `version` in `server.json` first, from this folder):
  1. `mcp-publisher validate`
  2. `mcp-publisher login dns --domain agentsai.in --private-key <private key in hex>`
  3. `mcp-publisher publish`
  4. Check: `curl "https://registry.modelcontextprotocol.io/v0.1/servers?search=in.agentsai/kanvi"`
- Raise the version and publish again when the server URL or the description changes.
