# Agent-Ready Directory

Find plumbers, HVAC (air conditioning and heating) companies and electricians in and around Austin, Texas.
Describe the problem and give a ZIP code or city ("my water heater is leaking, I'm in 78704"), and Claude returns
ranked local businesses with their phone numbers, websites, licenses and recent permitted work. The facts come from
public records (state licenses, city permits) and the businesses' own websites.

## What it contains

- **An MCP server reference:** `https://mcp.myownline.com/mcp`, the Agent-Ready Directory's hosted server. Its
  tools are `list_services`, `find_businesses`, `get_business`, `get_actions` and `report_issue`.
- **A skill** that tells Claude how to use these tools: Austin area and these three trades only, never guess a ZIP
  code, never invent a phone number or price, and say "may" when a fact is not confirmed.

The plugin runs nothing on your computer. It has no hooks, scripts or commands.

## Install (Claude Code)

```
/plugin marketplace add continuoustech-ai/agent-ready-plugins
/plugin install agent-ready-directory@agent-ready
```

## API key

The server needs a free API key (it starts with `ard_`). Get one at https://mcp.myownline.com/keys by signing in
with your email. When you enable the plugin, Claude Code asks for the key and stores it as a sensitive setting,
not in a file.

## What is sent, and where

Every tool call goes to `https://mcp.myownline.com/mcp` and nowhere else. It carries your API key (in the
`Authorization` header) and the tool's inputs: a trade or service, a ZIP code or city, a business name or id, and,
for `report_issue`, a short note about what is wrong with a listing. The server answers with business information
only. `report_issue` sends a report that a person checks; no other tool changes anything.

- Privacy notice: https://mcp.myownline.com/privacy
- Terms: https://mcp.myownline.com/terms
- Support and removal requests: support@myownline.com

## License

MIT
