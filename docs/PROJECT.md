# Inteagent Project Documentation

## 1. Executive Summary

Inteagent is a token launchpad built around a simple product primitive:

```text
TOKEN -> FEES -> AI AGENT -> ACTION
```

A creator does not launch an empty token page. The creator launches a token together with a declared mission. The mission is written as an agent prompt that describes how future fees should be used.

The product should make this relationship obvious within seconds:

- the token is the public asset;
- trading fees are the funding stream;
- the AI agent is the execution layer;
- the mission is the creator-defined policy;
- the activity log is the proof of execution.

Inteagent is not intended to be a generic meme-token launcher. Its product identity is infrastructure for programmable, transparent funding.

## 2. Product Principles

### Mission before speculation

The token launch flow presents the agent mission as a first-class field. A creator must explain the intended use of the fees instead of only entering a name and ticker.

### Transparency by default

Every agent should eventually expose its balance, received fees, executed actions and complete mission prompt. The interface should make an action understandable without requiring the user to trust hidden backend behavior.

### Non-custodial control

The platform must never request or store seed phrases or private keys. Wallets sign explicit transactions. Agent permissions must be limited to the funds and actions defined by the protocol.

### Prototype honestly

The current version is a frontend MVP. It uses realistic UI states and browser persistence, but it does not claim to execute real trades, route real fees or operate a real autonomous agent.

## 3. Target Users

### Creators

Creators launch a token around a cause, project, community or operating strategy. They define the token identity and write the agent mission.

### Supporters and traders

Users discover tokens by their missions. They can compare fee balances, agent status and recent actions before deciding whether a token is relevant to them.

### Mission operators

In a future production version, operators can review agent proposals, configure limits and pause execution when an action violates policy.

## 4. Main User Flows

### Launch a token

1. Open Launch.
2. Connect Phantom from a supported browser origin.
3. Enter token name, ticker and description.
4. Select an image and see its local preview.
5. Write an agent mission or use a prompt preset.
6. Review the live Agent Preview.
7. Launch the token.
8. See the new token in Discover and open its detail page.

In the current MVP, the final step creates a local record in the browser. In production, it should create and persist a protocol-backed token and agent configuration.

### Discover tokens

1. Open Discover.
2. Search token names, tickers, descriptions or missions.
3. Filter the registry.
4. Open a token card.
5. Review the agent balance, mission and activity.

The empty state is intentional: a new installation has no seeded tokens. The first creator populates the registry by launching a token.

### Inspect an agent

A token detail page displays:

- identity and ticker;
- market data when available;
- total fees generated;
- agent balance;
- mission prompt;
- action count;
- recent activity;
- network and transaction references in a production implementation.

## 5. Current Architecture

The repository is intentionally small and dependency-light:

```text
Inteagent/
|-- index.html
|-- styles.css
|-- app.js
|-- README.md
`-- docs/
    `-- PROJECT.md
```

### `index.html`

Provides the document shell, navigation, footer and success modal. It loads the stylesheet, Lucide icons and the application script.

### `styles.css`

Contains the visual language: black canvas, restrained green accent, thin borders, typography, responsive grids and mobile rules.

### `app.js`

Owns:

- hash routing;
- browser-local token registry;
- launch form state;
- prompt presets;
- image data URL preview;
- Discover search and filters;
- token detail rendering;
- Phantom detection and connection state;
- wallet address and Devnet balance display;
- storage synchronization between tabs.

## 6. Browser-Local Data Model

A token record currently follows this conceptual shape:

```js
{
  id,
  name,
  ticker,
  initials,
  description,
  image,
  mission,
  fees,
  balance,
  actions,
  marketCap,
  volume,
  holders,
  mint,
  signature,
  network,
  activity
}
```

The array is loaded from:

```text
localStorage key: inteagent-tokens
```

A new record starts with zero values:

- fees: `$0`
- balance: `$0`
- actions: `0`
- market cap: `$0`
- volume: `$0`
- holders: `0`

The image is stored as a browser data URL. This is suitable for a small prototype, but not for a production registry because data URLs increase browser storage usage and are not globally addressable.

## 7. Wallet Integration Boundary

Phantom is currently a permission gate for launching. The site:

- detects `window.phantom.solana`;
- calls Phantom from a user click;
- reads the selected public key;
- displays a shortened wallet address;
- requests a Devnet balance through Solana JSON-RPC;
- reacts to account changes.

The current browser-local launch does not require a blockchain transaction. This is deliberate: local persistence and on-chain deployment are separate responsibilities.

For a production launch flow, the wallet layer should:

1. build an explicit transaction;
2. show network and fee information;
3. request Phantom signature;
4. wait for confirmation;
5. save the confirmed address and signature;
6. never sign silently or use private keys in the page.

## 8. Production Architecture Proposal

A real product should be split into four layers.

### Web client

Responsible for forms, previews, wallet requests, token discovery and transaction status. It should never be the source of truth for balances or action history.

### API and database

Stores creator profiles, token configuration, agent prompts, metadata URLs, transaction references and indexed activity. It should validate inputs and enforce permissions server-side.

### On-chain programs and vaults

A token program creates the mint. A separate fee vault or treasury owns funds that the agent can use. The vault needs explicit constraints rather than unrestricted wallet access.

### Agent executor

A backend worker evaluates eligible actions, creates a proposed transaction and records the policy decision. Depending on the risk model, execution can require a human approval, a multisig signature or a bounded program instruction.

## 9. Metadata and Image Storage

The current image upload is local only. Production metadata should be uploaded before or during launch:

```json
{
  "name": "OpenCompute",
  "symbol": "COMPUTE",
  "description": "Funding open-source AI infrastructure.",
  "image": "ipfs://...",
  "external_url": "https://...",
  "agent": {
    "mission": "Use generated fees to purchase compute..."
  }
}
```

Recommended storage options:

- IPFS pinning service for content-addressed metadata;
- Arweave for long-lived permanent storage;
- a backend object store for mutable drafts and upload processing.

The final mint should reference a metadata URI rather than embedding a local browser image.

## 10. Agent Safety Model

An agent should not receive an unrestricted private key. A production policy should include:

- maximum spend per action;
- maximum spend per time window;
- approved recipient allowlist;
- approved asset and program allowlist;
- minimum balance reserve;
- simulation before execution;
- human approval for new action types;
- emergency pause;
- immutable action receipts;
- clear failure and retry states.

The creator prompt is intent, not sufficient authorization by itself. The protocol must translate intent into enforceable rules.

## 11. User-Facing States

The UI should cover these states explicitly:

- wallet unavailable;
- wallet available but disconnected;
- wallet connected;
- wrong network;
- insufficient fee balance;
- transaction awaiting signature;
- transaction submitted;
- transaction confirmed;
- transaction rejected;
- metadata upload failed;
- empty registry;
- search with no results;
- agent paused;
- agent action pending review;
- agent action completed.

A user should never see a successful launch before the required persistence or transaction step has completed.

## 12. Roadmap

### Phase 1: Current browser MVP

- static responsive interface;
- empty registry on first visit;
- local launch records;
- Phantom gate;
- image preview;
- discover and detail views.

### Phase 2: Durable web product

- React or Next.js application structure;
- backend API and database;
- authenticated creator dashboard;
- IPFS/Arweave metadata upload;
- token registry indexing;
- real transaction status pages.

### Phase 3: Protocol launch

- audited token and fee-vault programs;
- treasury permissions;
- bounded agent execution;
- Devnet test suite;
- mainnet deployment checklist;
- monitoring and incident response.

### Phase 4: Agent economy

- mission templates with policy validation;
- proposal and approval workflows;
- public execution receipts;
- agent performance analytics;
- community governance for shared missions.

## 13. Definition Of Done For Production Launch

Inteagent should not be presented as a real autonomous financial product until all of the following are true:

- token creation is confirmed on the intended network;
- metadata is globally retrievable;
- fee routing is enforced by an audited program;
- agent actions are bounded and observable;
- failures cannot silently spend funds;
- creators can pause or revoke execution;
- wallet signatures show exact transaction intent;
- activity history is independently verifiable;
- tests cover rejected signatures, wrong networks, duplicate launches and interrupted uploads;
- legal, security and operational review is complete.
