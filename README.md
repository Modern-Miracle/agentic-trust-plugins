# Agentic Trust Plugins

Public Claude Code plugin marketplace for **Agentic Trust** — Modern
Miracle's on-chain trust network for AI agents, built on Concordium.

This marketplace ships the **open verifier**: a thin Claude Code plugin that
wires any agent to the hosted Agentic Trust gateway, so its privileged calls
can be checked against MM-issued agent-identity and authority credentials at
call time — anonymously, with no key and no account.

The restricted issuance side (minting credentials) is a separate, MM-granted
private plugin and is **not** part of this repository. See
[Distribution model](#distribution-model) below.

## Installation

```
/plugin marketplace add Modern-Miracle/agentic-trust-plugins
/plugin install agent-gateway@agentic-trust
```

## Plugins

### agent-gateway (v0.1.0)

Wires Claude Code to the hosted Agentic Trust gateway
(`https://agents.modernmiracle.org/mcp`) over remote streamable-HTTP MCP. No
local process, no configuration, no auth. See
[`plugins/gateway/README.md`](plugins/gateway/README.md) for what the
gateway verifies and how it fits the trust network.

## Distribution model

Modern Miracle operates two asymmetric surfaces on purpose (restricted issuer
/ open verifier, the CA / eIDAS-trusted-list pattern):

- **Issuance ("the mint")** mints CIS-8004 agent identities and CIS-4
  `AgentAuthorityCredential`s under Modern Miracle's sole issuer key.
  Distribution is restricted to MM and authorised partners via a private
  Claude Code marketplace — not this one.
- **Verification ("the gateway")**, shipped here, checks and enforces those
  credentials, including revocation. Anyone may install and run it. Every
  independent deployment widens enforcement coverage without touching the
  issuer's signing key or weakening the network.

## Contributing

Issues and PRs are welcome. CI validates `marketplace.json` and every
`plugin.json` against their published JSON Schemas, validates the shape of
each plugin's `.mcp.json`, scans for committed secrets, checks markdown
links, and smoke-tests reachability of the hosted gateway referenced by the
`agent-gateway` plugin.

## License

MIT. See [LICENSE](LICENSE).

## Maintainer

Modern Miracle — mak@modern-miracle.com
