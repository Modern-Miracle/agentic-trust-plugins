# Vetting rationale — catalogue v1.0.0

**Status: research-preview seed catalogue.** This is the initial vocabulary
behind credentials issued by the Agentic Trust registry on the Concordium
testnet. It was vetted by Modern Miracle founder review on 2026-07-12 — a
single-issuer review, not an independent audit. It exists so that every
vetting decision behind an issued credential is publicly inspectable and
contestable, at the maturity the project actually has today.

## The four capability flags

Every tool carries exactly four boolean capability flags, chosen because each
maps to a concrete EU AI Act transparency or prohibition surface:

| Flag                                              | AI Act surface                                                 |
| ------------------------------------------------- | -------------------------------------------------------------- |
| `produces_synthetic_image`                        | Art. 50(2) synthetic-content marking                           |
| `produces_synthetic_audio_video_text`             | Art. 50(2) synthetic-content marking                           |
| `publishes_public_interest_text`                  | Art. 50(4) public-interest text disclosure                     |
| `emotion_recognition_or_biometric_categorisation` | Art. 50(3) disclosure, adjacent to Art. 5 prohibited practices |

**`emotion_recognition_or_biometric_categorisation` is deliberately `false`
on every tool in this catalogue.** Emotion recognition and biometric
categorisation sit adjacent to the EU AI Act Art. 5 prohibited-practice
perimeter, and this registry excludes that entire capability family by
absence from the vocabulary: no tool that would set this flag has been
admitted, so no credential can attest it. Its presence as a flag (rather
than its omission) is intentional — a verifier can confirm the exclusion is
a decision, not an oversight.

## Per-tool rationale

### claims-lookup — operational / low

Reads a claim record by its reference number. The record is business-process
data about a claim's state, not a person's profile, so it is classed
`operational` rather than `personal` — the tool surface returns claim-state
fields, not identity fields. Risk is `low`: read-only, no decision authority,
no outbound side effects. All four capability flags are false; the tool
generates nothing and publishes nothing.

### policy-lookup — operational / low

Reads the cover details of an insurance policy. Cover terms (limits,
exclusions, product configuration) are operational business data; the tool
does not expose the policyholder's identity attributes, which is why it is
not `personal`. Risk is `low` for the same reasons as claims-lookup:
read-only and side-effect-free. All capability flags are false.

### payment-status — financial / medium

Checks whether a payment has been sent or received. Payment state is
`financial` data by nature: it reveals money movement between parties. Risk
is `medium` rather than low because financial visibility is a meaningful
escalation over operational reads — it can inform fraud, and its misuse has
direct monetary relevance — but the tool remains read-only with no authority
to move funds. All capability flags are false.

### customer-profile — personal / high

Reads a customer's name and contact details. This is GDPR personal data in
the plainest sense, hence the `personal` data class. Risk is `high` because
identity-plus-contact data is the raw material of social engineering and
unlawful profiling, and any credential granting it must be narrowly and
visibly scoped. All capability flags are false: reading a profile generates
no synthetic content and publishes nothing.

### claim-decision — personal / high

Approves or declines a claim that affects a real person. The decision is
about a natural person and directly produces legal or similarly significant
effects on them, so the data class is `personal` and the risk is `high` —
this is the GDPR Art. 22 automated-decision surface, and the surrounding
gateway policy (not this catalogue) is what enforces human oversight. The
catalogue's job is to make it visible that a credential carried this
authority. All capability flags are false; deciding is not generating or
publishing content.

### health-record — special-category / high

Reads health information attached to a claim. Health data is GDPR Art. 9
special-category data by definition, which fixes the data class; no lesser
class is truthful. Risk is `high` as the floor for any special-category
access — even read-only. All capability flags are false.

### send-message — operational / low

Sends a message to the counterparty organisation. This is
organisation-to-organisation process communication, so the data class is
`operational`. Risk is `low`: it has an outbound side effect, but the
recipient is a fixed counterparty organisation rather than the public or an
arbitrary address, which is what keeps it out of medium. All capability
flags are false — in particular `publishes_public_interest_text` is false
because a bilateral B2B message is not publication to the public.

### nft-mint — operational / medium

Generates and mints a synthetic image as an on-chain token. The data it
handles (token metadata, mint parameters) is `operational`; it touches no
personal or financial records. Risk is `medium` because minting is an
irreversible on-chain write with a generated artefact attached.
`produces_synthetic_image` is **true** because the tool's core function is
producing an AI-generated image — this is exactly the Art. 50(2) marking
surface the flag exists to expose, and any verifier of an nft-mint-bearing
credential can see the marking obligation attaches. The other three flags
are false: no audio/video/text generation, no public-interest publication,
no biometric capability.

### content-generate — operational / medium

Produces AI-written text/audio/video for publication on public-interest
matters. The tool operates on content pipelines, not personal records, hence
`operational`; risk is `medium` because publication is an outbound,
reputationally consequential side effect short of decision authority over a
person. `produces_synthetic_audio_video_text` is **true** because the tool
emits AI-generated text, audio, and video (Art. 50(2) marking).
`publishes_public_interest_text` is **true** because that output is destined
for publication on matters of public interest, which is the Art. 50(4)
disclosure surface. The remaining two flags are false.

## Archetype note: ORIGINAL

The `ORIGINAL` archetype appears in the taxonomy but is **refused at
issuance**: its Art. 50(2) marking obligations are deferred per ADR-16
OQ-10, and no registered tool derives it. It is published here so the
refusal itself is on the public record — a verifier who encounters an
`ORIGINAL` credential should treat it as invalid against catalogue v1.0.0.

## Vetting standard going forward

New tools enter the catalogue only via issuer review of the implementing
counterparty's actual tool surface, against this checklist:

1. **Data-class truthfulness** — the declared data class matches what the
   tool surface actually reads or writes, at the most sensitive class it
   touches.
2. **Risk level** — read-only operational surfaces start at `low`;
   financial visibility, irreversible writes, or content publication raise
   to `medium`; personal-data access, special-category access, or decision
   authority over a person raise to `high`.
3. **The four capability flags** — each flag is assessed against the tool's
   real behaviour, not its description; `emotion_recognition_or_biometric_categorisation`
   remains excluded by policy.

Every admission ships with its rationale published in the corresponding
version's `VETTING.md`, in a new immutable catalogue version. A tool whose
surface cannot be reviewed is not admitted.
