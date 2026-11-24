# Kanvi

Kanvi, from AgentsAI.

Find plumbers, HVAC (air conditioning and heating) companies and electricians in
Texas. Describe the problem or the service and a ZIP code, city or neighbourhood ("my water
heater is leaking, I'm in 78704", "half my outlets went dead, 78745"), and Claude returns ranked local businesses with their
phone numbers, websites, booking links, licenses and recent permitted work. The facts come from
public records (state licenses, city permits) and the businesses' own websites.

## What it contains

- **An MCP server reference:** `https://api.kanvi.agentsai.in/mcp`, Kanvi's hosted server. Its
  tools are `list_services`, `find_businesses`, `get_business`, `get_actions` and `report_issue`.
- **A skill** that tells Claude how to use these tools: `SKILL.md` with the rules for every trade (Texas only and
  the directory's trades only, emergencies first, ask for a place only when a search needs one, never invent a
  phone number, price or hours, say "may" when a fact is not confirmed), and one file per kind of business, read
  only when needed: `home-services.md` (plumbing, HVAC, electrical) and `visit-businesses.md` (salons, barbers).

The plugin runs nothing on your computer. It has no hooks, scripts or commands.

## Install (Claude Code)

```
/plugin marketplace add continuoustech-ai/agent-ready-plugins
/plugin install kanvi@agents-ai
```

## API key

The server needs a free API key (it starts with `ard_`). Get one at https://api.kanvi.agentsai.in/keys by signing in
with your email. When you enable the plugin, Claude Code asks for the key and stores it as a sensitive setting,
not in a file.

## What is sent, and where

Every tool call goes to `https://api.kanvi.agentsai.in/mcp` and nowhere else. It carries your API key (in the
`Authorization` header) and the tool's inputs: a trade or service, a ZIP code or city, a business name or id, and,
for `report_issue`, a short note about what is wrong with a listing. The server answers with business information
only. `report_issue` sends a report that a person checks; no other tool changes anything.

- Privacy notice: https://api.kanvi.agentsai.in/privacy
- Terms: https://api.kanvi.agentsai.in/terms
- Support and removal requests: support@myownline.com

## License

MIT
