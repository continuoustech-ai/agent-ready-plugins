# Kanvi

Kanvi, from AgentsAI.

Find plumbers, HVAC (air conditioning and heating) companies, electricians, hair and nail salons and barbers in
Texas. Describe the problem or the service and a ZIP code, city or neighbourhood ("my water
heater is leaking, I'm in 78704", "highlights near 78745"), and Claude returns ranked local businesses with their
phone numbers, websites, booking links, licenses and recent permitted work. The facts come from
public records (state licenses, city permits) and the businesses' own websites.

## What it contains

- **An MCP server reference:** `https://api.agentsai.in/mcp`, AgentsAI's hosted server. Its
  tools are `list_services`, `find_businesses`, `get_business`, `get_actions` and `report_issue`.
- **A skill** that tells Claude how to use these tools: `SKILL.md` with the rules for every trade (Texas only and
  the directory's trades only, emergencies first, ask for a place only when a search needs one, never invent a
  phone number, price or hours, say "may" when a fact is not confirmed), and one file per kind of business, read
  only when needed: `home-services.md` (plumbing, HVAC, electrical) and `visit-businesses.md` (salons, barbers).

- **Setup files for other apps**, not used by Claude Code: `.antigravity-plugin/plugin.json` and `mcp_config.json`
  (Antigravity; it sends your key from the `ARD_API_KEY` setting) and `plugin.json` and `mcp.json` (Codex). They
  point at the same server and send the same key, nowhere else.

The plugin runs nothing on your computer. It has no hooks, scripts or commands.

## Install (Claude Code)

```
/plugin marketplace add continuoustech-ai/agent-ready-plugins
/plugin install kanvi@agents-ai
```

## API key

The server needs a free API key (it starts with `ard_`). Get one at https://api.agentsai.in/keys by signing in
with your email. When you enable the plugin, Claude Code asks for the key and stores it as a sensitive setting,
not in a file.

## What is sent, and where

Every tool call goes to `https://api.agentsai.in/mcp` and nowhere else. It carries your API key (in the
`Authorization` header) and the tool's inputs: a trade or service, a ZIP code or city, a business name or id, and,
for `report_issue`, a short note about what is wrong with a listing. The server answers with business information
only. `report_issue` sends a report that a person checks; no other tool changes anything.

- Privacy notice: https://api.agentsai.in/privacy
- Terms: https://api.agentsai.in/terms
- Support and removal requests: support@myownline.com

## License

MIT
