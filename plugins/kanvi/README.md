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
  only when needed, such as `home-services.md` (plumbing, HVAC, electrical).

The plugin runs nothing on your computer. It has no hooks, scripts or commands.

## Install (Claude Code)

```
/plugin marketplace add continuoustech-ai/agent-ready-plugins
/plugin install kanvi@agents-ai
```

## Sign in

The server needs a free Kanvi account; no API key. The first time, Claude shows the server as needing sign-in:

- **Claude Code:** `/mcp`, select `plugin:kanvi:kanvi`, **Authenticate**.
- **Claude apps (web, Desktop, mobile):** connect Kanvi in Settings > Connectors.

Your browser opens; sign in with your email, accept the terms and allow the app. Claude keeps the sign-in token, not
a password.

Developers who prefer an API key (`ard_...`, from https://api.kanvi.agentsai.in/keys) can add the server themselves
instead; see the repository README.

## What is sent, and where

Every tool call goes to `https://api.kanvi.agentsai.in/mcp` and nowhere else. It carries your sign-in token (in the
`Authorization` header) and the tool's inputs: a trade or service, a ZIP code or city, a business name or id, and,
for `report_issue`, a short note about what is wrong with a listing. The server answers with business information
only. `report_issue` sends a report that a person checks; no other tool changes anything.

Sign-in itself goes through Supabase Auth, which runs Kanvi's accounts
(`https://fidbeemzahxomqsgmcow.supabase.co/auth/v1`), and the consent page on `https://api.kanvi.agentsai.in`.
Your email is sent there to sign you in; nothing else.

- Privacy notice: https://api.kanvi.agentsai.in/privacy
- Terms: https://api.kanvi.agentsai.in/terms
- Support and removal requests: https://kanvi.agentsai.in/contact

## License

MIT
