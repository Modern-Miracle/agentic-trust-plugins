# Changelog

All notable changes to this marketplace and its plugins are documented in
this file. The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this marketplace adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [0.1.0] - 2026-07-10

### Added

- Initial marketplace: `agentic-trust`.
- `agent-gateway` plugin (v0.1.0) — thin remote-MCP wiring for the hosted
  Agentic Trust enforcement gateway (open verifier, anonymous, rate-limited,
  no auth).
- CI: JSON-schema validation of `marketplace.json` and `plugin.json`,
  `.mcp.json` shape validation, gateway reachability smoke test, gitleaks
  secret scan, markdown link check.
