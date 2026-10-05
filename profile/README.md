<p align="center">
  <img src="./logo.png" alt="Klap" width="96" />
</p>

<h1 align="center">Klap</h1>
<p align="center"><strong>Keep Liquidity Always Permissionless.</strong></p>

<p align="center">
  Crypto payments without red tape, for everyone.<br />
  A non-custodial payments API, and a growing suite of products built on it.
</p>

<p align="center">
  <a href="https://klappay.com">Website</a> ·
  <a href="https://docs.klappay.com">Docs</a> ·
  <a href="https://api.klappay.com">API reference</a> ·
  <a href="https://app.klappay.com">Dashboard</a>
</p>

---

Funds never pass through Klap. Every charge gets a predicted, immutable
on-chain address (a [0xSplits](https://splits.org) v2 split contract
with its recipients frozen at creation), and the payer pays straight
into it. Klap detects the transfer and triggers the split, which pays
the merchant and the platform fee on-chain in one transaction. Our
operator wallet only ever pays the gas for that; it can't redirect
funds.

USDC and USDT across eight networks: Base, Arbitrum, Optimism, Polygon,
Ethereum, Avalanche, BNB Chain, and [Arc](https://arc.network). TRON
and Solana are next.

## Products

| Product | What it is | Status |
|---|---|---|
| **Klap Core** | The payments API everything else runs on: charges, webhooks, real-time status over SSE, sandbox, analytics, swap-to-pay, escrow, fee passthrough. | Live |
| **[Klap App](https://app.klappay.com)** | Merchant dashboard: API keys, teams, charges, webhook deliveries, distributions. | Live |
| **Klap Checkout** | Hosted checkout page for any charge. Pay with a connected wallet, a QR code, or a raw address, with live status. | Building |
| **Klap Link** | Payment links and product pages with no code: one-time links, stock, buyer details, receipts, and monthly/yearly subscriptions (push-only, the payer approves every renewal). | Building |
| **Klap One** | One button, one identity for paying from any of your wallets. OTP sign-in, linked wallets, and approvals that never leave the payer's own wallet app. Never holds a key. | Building |
| **Klap Trust** | Per-wallet trust score from Klap's cross-merchant payment history, with a self-service page where a wallet owner signs in to see their own score. | Building |
| **Klap Fund** | Goal-and-deadline crowdfunding. Hit the goal and funds release; miss it and contributors get refunded. | Building |
| **Klap Give** | Direct support for creators, developers, open-source projects, and causes, with an optional goal. | Building |

## Build with Klap

Everything you need to integrate is open source:

| Package | What it does | Docs |
|---|---|---|
| [`@klappay/node`](https://www.npmjs.com/package/@klappay/node) · [repo](https://github.com/klappay/klap-node) | Official Node.js SDK: charges, `waitForConfirmation`, webhook verification, recipients, networks, metrics. | [node-sdk.klappay.com](https://node-sdk.klappay.com) |
| [`@klappay/types`](https://www.npmjs.com/package/@klappay/types) | TypeScript types and Zod schemas for the API's full request/response surface. Framework-agnostic, zero networking. | [api.klappay.com/types](https://api.klappay.com/types) |
| [`@klappay/cli`](https://www.npmjs.com/package/@klappay/cli) · [repo](https://github.com/klappay/klap-cli) | `klap` in your terminal: create charges, simulate sandbox events, and stream webhooks to `localhost` with `klap listen`, no tunnel needed. | [cli.klappay.com](https://cli.klappay.com) |
| [`@klappay/checkout-kit`](https://www.npmjs.com/package/@klappay/checkout-kit) · [repo](https://github.com/klappay/klap-checkout-kit) | Build your own checkout UI without redoing wallet integration or the charge-to-payment-option logic. Examples for Hono, Next.js, SvelteKit, and Nuxt. | [node-checkout-sdk.klappay.com](https://node-checkout-sdk.klappay.com) |
| [`@klappay/one`](https://www.npmjs.com/package/@klappay/one) · [repo](https://github.com/klappay/klap-one-js) | Klap One's drop-in pay button: a `<klappay-button>` web component, a `data-` attribute, or React. | [js-one.klappay.com](https://js-one.klappay.com) |

The unified docs at **[docs.klappay.com](https://docs.klappay.com)**
([repo](https://github.com/klappay/klap-docs)) cover each resource end
to end: the concept, the REST endpoint, the SDK call, and the schema.
Every docs site also publishes `llms.txt` and `llms-full.txt` for
agents.

```ts
import { createClient } from '@klappay/node'

const klap = createClient() // reads KLAP_API_KEY and KLAP_BASE_URL

const charge = await klap.charges.create({
  amount: 49.9,
  acceptedPayments: [{ token: 'USDC', network: 'base' }],
})

const confirmed = await charge.waitForConfirmation()
```

## How a payment works

1. **Create a charge.** Klap predicts (but doesn't deploy yet) an
   immutable split address via CREATE2. It's the same address whichever
   accepted token/network pair the payer ends up using.
2. **The payer sends funds** straight to that address. No custody, no
   intermediate wallet.
3. **Klap detects and settles.** The transfer is picked up in real time,
   with a reconciliation fallback, and the split distributes it
   on-chain to the merchant and the platform fee in one transaction.

## On-chain stopgaps

When 0xSplits isn't on a network yet, we run its own audited contracts
there ourselves rather than writing new split logic:

- **[klap-tron](https://github.com/klappay/klap-tron)**: 0xSplits v2 on
  TRON, with one documented patch for TVM's `CREATE2` prefix, to bring
  TRC-20 USDT in.
- **[klap-arc](https://github.com/klappay/klap-arc)**: 0xSplits v2 on
  Arc. Deprecated now that 0xSplits ships Arc officially; kept for the
  record and the migration runbook.

---

<p align="center"><sub>The Klap API and products are closed source. The
SDKs, types, CLI, checkout kit, pay button, docs, and on-chain stopgaps
listed above are public.</sub></p>
