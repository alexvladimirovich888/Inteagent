# Inteagent

Inteagent is a browser-first token launchpad where every token has an autonomous mission. A creator defines the mission as a prompt, and the token becomes the entry point for an agent-led treasury.

```text
TOKEN -> FEES -> AI AGENT -> ACTION
```

The current repository is a frontend MVP. It demonstrates the complete product experience without pretending that token deployment, wallet custody, fee routing, or AI execution are already production-ready.

## Product Concept

Inteagent changes the role of a token from a purely speculative asset into a programmable funding mechanism:

1. A creator defines a token.
2. The creator writes the mission of its AI agent.
3. Trading activity is expected to generate fees.
4. The agent is responsible for applying those fees to the declared mission.
5. Every action should eventually be visible in a public activity record.

Example mission:

> Use generated fees to fund open-source AI infrastructure. Prioritize compute, developer grants and public goods. Keep a transparent record of every action.

## Current MVP

The current implementation is a static HTML/CSS/JavaScript application with hash-based routing.

Implemented:

- Landing page explaining the product model
- Token launch form
- Token name, ticker and description fields
- Agent mission prompt editor
- Prompt presets
- Live agent preview
- Token image selection and local preview
- Phantom connection gate before launch
- Browser-local token registry using `localStorage`
- Discover page with search and filters
- Token detail pages
- Zero-balance launch state for newly created tokens
- Persistence after refresh and across tabs on the same browser origin
- Responsive desktop and mobile layout

The launch action currently creates a browser-local token record. It does not deploy a token to Solana and does not move funds.

## Run Locally

Node.js is not required for this MVP. Run a static HTTP server from the repository root:

```powershell
python -m http.server 5500
```

Open:

```text
http://localhost:5500/index.html#/launch
```

Use HTTP rather than opening `index.html` directly with `file://`. Browser wallet extensions require a web origin.

## Deploy To Vercel

1. Import `alexvladimirovich888/Inteagent` into Vercel.
2. Select framework preset `Other`.
3. Leave Build Command empty.
4. Leave Output Directory empty.
5. Deploy the repository root.

The app is static and does not need a build step.

## Wallet Notes

Phantom must be installed in the regular Chrome or Brave browser. The integrated VS Code browser cannot access Chrome extensions.

The current wallet connection is used as a launch permission gate. The browser-local registry remains local to the deployed origin and browser profile. A token created on localhost will not automatically appear on the Vercel domain.

## Project Files

- `index.html` - application shell, navigation, modal and external icon/font imports
- `styles.css` - visual system and responsive layout
- `app.js` - routing, token registry, launch flow, wallet integration and UI behavior
- `docs/PROJECT.md` - detailed product and technical specification

## Security Boundary

This repository is a prototype. Never put seed phrases, private keys, API secrets or custodial signing logic in frontend code. Production wallet actions must be explicit transactions presented to the user for signing in Phantom.

## Roadmap

- Replace `localStorage` with a database-backed token registry
- Upload token images and metadata to IPFS or Arweave
- Deploy a real token program and document the exact contract behavior
- Add a backend indexer for token and fee activity
- Define an auditable fee vault and agent permission model
- Add human approval limits, allowlists and emergency pause controls
- Add real-time activity and transaction links
- Add authentication and creator dashboards
- Add tests for wallet states, launch validation and data persistence
