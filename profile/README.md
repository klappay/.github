<p align="center">
  <img src="./logo.png" alt="Klap" width="96" />
</p>

<h1 align="center">Klap</h1>
<p align="center"><strong>Keep Liquidity Always Permissionless.</strong></p>

<p align="center">
  A non-custodial API for accepting and distributing on-chain crypto payments.
</p>

---

Klap funds never pass through Klap. Every charge gets a predicted,
immutable on-chain address — a [0xSplits](https://splits.org) split
contract with recipients frozen at creation — and payments settle
directly from payer to merchant on-chain. Our own operator wallet only
ever pays the gas to trigger an already-deployed, already-immutable
split; it can't redirect funds.

## What's here

- **Core API** — charges, webhooks, real-time payment status over
  Server-Sent Events, and a full sandbox for testing without real
  transactions. USDC and USDT across seven networks: Base, Arbitrum,
  Optimism, Polygon, Ethereum, Avalanche, and BNB Chain.
- **[`@klappay/types`](https://www.npmjs.com/package/@klappay/types)** —
  TypeScript types and Zod schemas for the API's full request/response
  surface. MIT-licensed, framework-agnostic, zero networking.
- **Official SDKs and a CLI** — client libraries for integrating without
  hand-rolling HTTP calls and webhook signature verification, and a
  terminal client for creating charges, simulating sandbox events, and
  forwarding webhooks straight to `localhost`.

## How a payment works

1. **Create a charge.** Klap predicts (doesn't deploy yet) an immutable
   split contract address via CREATE2 — the same address regardless of
   which accepted token/network pair the payer ends up using.
2. **Payer sends funds.** USDC or USDT, on any network the charge
   accepts, straight to that address — no custody, no intermediate
   wallet.
3. **Klap detects and settles.** The transfer is picked up in real time
   (webhook-driven, with a reconciliation fallback), and the split
   contract distributes the funds on-chain: merchant and platform fee,
   in one transaction.

## Integration surface

- **Webhooks** — a full event vocabulary across payments, account,
  webhook-delivery, and security categories, HMAC-signed and retried on
  failure.
- **Real-time (SSE)** — stream a charge's status live instead of
  polling.
- **Sandbox** — simulate any charge or account event with a `test` API
  key, no real transactions required.
- **Multi-organization accounts** — a user can belong to more than one
  organization, each with its own role and API keys.

Full API reference, integration guides, and package docs live alongside
each package's own repository.

---

<p align="center"><sub>Klap is closed-source at its core — the API
implementation isn't public — but the packages that make integrating
against it easier (<code>@klappay/types</code>, the official SDKs, and
the CLI) are.</sub></p>
