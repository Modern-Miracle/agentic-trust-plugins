# Security Policy

## Reporting a vulnerability

Please report suspected vulnerabilities in this marketplace, the
`agent-gateway` plugin, or the hosted gateway endpoint it wires
(`https://agents.modernmiracle.org/mcp`) privately:

- Email: **mak@modern-miracle.com** (subject: `SECURITY — agentic-trust-plugins`)
- Or use GitHub's private vulnerability reporting on this repository
  ("Report a vulnerability" under the Security tab), if enabled.

Do **not** open a public issue for security reports. You should receive an
acknowledgement within 72 hours.

## Scope notes

- **The endpoint is anonymous by design.** The hosted gateway's `/mcp`
  surface accepts unauthenticated, rate-limited requests on purpose — it is
  the open **verifier** half of the Agentic Trust dual (ADR-70: restricted
  issuer / open verifier). "The MCP endpoint answers without a token" is
  expected behavior, not a finding. The gateway can only *verify* credentials;
  it cannot mint or revoke them.
- **What we do care about:** anything that lets a caller bypass credential
  verification or revocation checks, escape the rate limit at scale, tamper
  with this repo's supply chain (the pinned `.mcp.json` URL, CI action pins,
  checksum-verified tool downloads), or impersonate the issuer.

## Supply-chain posture of this repo

- `main` is protected: changes land via pull request, force pushes and
  deletions are blocked, and CI status checks are required.
- CI pins every GitHub Action to a full commit SHA and verifies downloaded
  binaries (gitleaks) against their official sha256 checksums.
- CI asserts the plugin's `.mcp.json` URL is **exactly**
  `https://agents.modernmiracle.org/mcp` — a PR substituting another host
  fails.
