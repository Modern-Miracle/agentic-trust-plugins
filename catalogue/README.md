# Agentic Trust — public tool catalogue

This directory is the **public, versioned vocabulary behind every credential
issued by the Agentic Trust registry**. Each `AgentAuthorityCredential`
minted by Modern Miracle attests an archetype plus a subset of vetted tools;
those identifiers only mean something if anyone — not just keyed clients —
can resolve them. This catalogue is that resolution surface: a transparency
log of what the registry's vocabulary was at each version, and why each
entry was admitted.

**Verifiers resolve credential meaning against the catalogue version in
force at issuance.** A credential minted while v1.0.0 was current is
interpreted against `v1/catalogue.json`, regardless of what later versions
add.

## Contents

| File                | Purpose                                                                           |
| ------------------- | --------------------------------------------------------------------------------- |
| `v1/catalogue.json` | Machine-readable catalogue: archetypes, data classes, tools with capability flags |
| `v1/VETTING.md`     | Per-tool vetting rationale and the vetting standard for admission                 |

## Versioning

Versions follow semver, mirrored as one directory per major version line:

- **Any tool addition, removal, or change of any field creates a new
  catalogue version** (new `catalogue_version`, and a new version directory
  for a new major line).
- **Prior versions are immutable.** Once a version's hash has been anchored,
  its files are never edited — corrections happen in a new version. A change
  to an already-anchored `catalogue.json` is, by definition, a compromise
  signal.

## Canonical form and hashing

`catalogue.json` is hashed byte-for-byte, so its serialisation is fixed:
UTF-8, LF line endings, 2-space indentation, a single trailing newline, and
the key order as committed. **The committed bytes are the canonical form** —
do not reformat, re-indent, or re-serialise the file; any such change alters
the hash and is a new version by the rule above. Verify with:

```
shasum -a 256 catalogue/v1/catalogue.json
```

## Signing

Two layers bind each version to the issuer:

1. **GPG-signed commits** — every commit touching `catalogue/` is signed by
   the Modern Miracle maintainer key, so the git history itself attests who
   published each version.
2. **Per-version sha256** — the hash of each version's `catalogue.json` is
   recorded below and anchored on the Concordium chain, so the catalogue's
   integrity does not depend on trusting this repository's host.

## Anchoring

### v1.0.0

```
sha256(catalogue/v1/catalogue.json) = a7960e4d383749290e0e6a560a1e8ff1342d208aa43a05ef9b876bdb022221e1
```

Chain anchor (Concordium testnet registerData tx):
`351c7842d3ec624b382da741727c65892dc8364c4d33cf7bec69ef426c8bce28` (finalized
in block `fc86fa20094119f8b2544cacd353d423bd9f34132fd0b841927fdcd0261fcee9`,
2026-07-12). Anchor envelope format (52 bytes, hex-encoded on chain): the
ASCII prefix `MM-CATALOGUE-v1.0.0:` followed by the raw 32-byte sha256 of
`catalogue/v1/catalogue.json` — so any verifier can locate the anchor by
prefix and compare the trailing 32 bytes against the committed file's hash
