# agent-gateway

The **open verifier** half of the Agentic Trust dual (see [ADR-69](#the-dual) /
[ADR-70](#the-dual)). This plugin wires Claude Code to Modern Miracle's hosted
enforcement gateway so any agent's privileged calls can be checked, at call
time, against MM-issued **CIS-4 AgentAuthorityCredential** and **CIS-8004**
agent-identity records on the Concordium blockchain — including live
revocation.

It is intentionally thin: one remote MCP server entry, nothing else. There is
no local process, no build step, and no configuration to fill in.

## Install

```
/plugin marketplace add Modern-Miracle/agentic-trust-plugins
/plugin install agent-gateway@agentic-trust
```

That's it. On enable, Claude Code connects to the hosted gateway
(`https://agents.modernmiracle.org/mcp`) over streamable HTTP.

## Anonymous, rate-limited, no account

- **No API key.** No OAuth. No signup. The gateway is reachable by anyone who
  installs the plugin.
- **Rate-limited.** The anonymous tier is throttled to keep the shared service
  fair; a paid/authenticated tier with higher limits is on the roadmap but not
  required to use the gateway today.
- **Verification only.** This plugin — and the gateway it talks to — can check
  and enforce credentials. It cannot mint or revoke them. Issuance stays
  behind Modern Miracle's restricted, key-custodied mint (W1); it is not part
  of this public marketplace.

## What "verify" means here

For each privileged call your agent makes, the gateway:

1. Resolves the calling agent's on-chain identity (CIS-8004).
2. Checks it holds a valid, unrevoked CIS-4 `AgentAuthorityCredential` issued
   by Modern Miracle.
3. Enforces the call against that credential's scope and admits or denies
   accordingly.

Because every credential traces back to a single accountable issuer (Modern
Miracle), independently deployed gateways don't fragment trust — they widen
where that trust is checked, at zero marginal cost to the network.

## The dual

Modern Miracle operates two asymmetric surfaces by design:

| Surface | Role | Distribution | Marketplace |
|---|---|---|---|
| W1 — issuance ("the mint") | Mints CIS-8004 + CIS-4 credentials under MM's sole issuer key | Restricted — MM-granted access only | Private MM marketplace (not this repo) |
| W2 — gateway ("the verifier") | Verifies + enforces credentials, including revocation | Open — anyone may deploy | **This repo** |

This is the CA / eIDAS-trusted-list pattern: one accountable issuer, unlimited
verifiers. Giving the verifier away for free is what makes an MM-issued
credential worth holding — the open gateway is the razor, the restricted mint
is the blades. See the registry's ADR-69 (sole credential issuer) and ADR-70
(distribution model: private mint, public gateway) for the full decision
record.

## Status

The gateway's anonymous tier is being rolled out. If `/mcp` calls return
`401` today, the anonymous tier has not yet shipped on the hosted endpoint —
this is tracked and does not require any change on your side; the plugin will
start working the moment the endpoint goes live. `/health` is already live
and returns `200`.

## License

MIT. See the repository root [LICENSE](../../LICENSE).
