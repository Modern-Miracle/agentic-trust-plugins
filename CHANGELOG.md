# Changelog

All notable changes to this marketplace and its plugins are documented in
this file. The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this marketplace adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [0.1.1] - 2026-07-11

### Added

- `SECURITY.md` — private reporting channel; documents that the gateway
  endpoint is anonymous by design and the repo's supply-chain posture.
- `CODEOWNERS` — maintainer review on the MCP wiring, CI, and marketplace
  manifest.
- Classic branch protection on `main` (PR required, force pushes and
  deletions blocked, CI status checks required).

### Changed

- CI: `.mcp.json` check tightened from scheme-only to **exact URL equality**
  (`https://agents.modernmiracle.org/mcp`) — a host substitution now fails CI.
- CI: every GitHub Action pinned to a full commit SHA; gitleaks download
  verified against its official sha256 checksum before execution.
- CI: gateway reachability job now asserts the anonymous MCP `initialize`
  POST returns `200` (the anonymous tier is live) and is marked
  informational (`continue-on-error`) so live-endpoint flakiness cannot
  block merges.
- Gateway plugin README: stale "may still return 401" status replaced —
  the anonymous tier is live.

## [0.1.0] - 2026-07-10

### Added

- Initial marketplace: `agentic-trust`.
- `agent-gateway` plugin (v0.1.0) — thin remote-MCP wiring for the hosted
  Agentic Trust enforcement gateway (open verifier, anonymous, rate-limited,
  no auth).
- CI: JSON-schema validation of `marketplace.json` and `plugin.json`,
  `.mcp.json` shape validation, gateway reachability smoke test, gitleaks
  secret scan, markdown link check.
