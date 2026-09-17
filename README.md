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

## Product Boundary

Inteagent is a frontend product prototype. Its browser registry demonstrates the product model and user flows; it is not a blockchain ledger and it does not execute financial operations.

The detailed product specification is in [docs/PROJECT.md](docs/PROJECT.md).
