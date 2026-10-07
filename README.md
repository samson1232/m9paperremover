# m9paperremover — KYC / Advisory Document Audit Skill

Class-level skill for auditing a life-insurance policy document against the Singapore LIA
Members' Undertaking MU 20/15 (Minimum Standard for Life Insurance Advisory Process) and FAA-N16,
surfacing KYC / suitability / compliance findings, and drafting a formal complaint requesting a
reversal of the transaction.

**Live repo:** https://github.com/samson1232/m9paperremover

## What it does

- Cross-references a policy contract, application, facts report and product illustration against
  MU 20/15's "Know Your Client" (§3.9), "Needs Analysis" (§3.10) and "Representative's
  Recommendation" (§3.11) sections.
- Scores findings MVI / V / LVI and maps each to a regulatory basis (MU 20/15 §x.y, FAA-N16,
  COLIP, Insurance Act s.23(5)).
- Drafts a formal complaint requesting full reversal (refund of premiums without interest,
  rescission of the policy, return to pre-application position) with delivery/channel routing.

## Usage

Run the 7-step procedure (extract → KYC cross-ref → needs analysis → recommendation →
suitability flags → map and draft → verify), using the reference files:

- `references/delivery-matrix.md` — complaint / regulatory / contact routing.
- `references/analysis-template.md` — Claim / Evidence / Regulatory basis table.
- `references/complaint-template.md` — fill-in complaint draft.

## Files

- `SKILL.md` — the workflow and the redacted findings example.
- `references/*.md` — supporting tables and templates.

## Provenance

Built from a real KYC audit of a life-insurance policy issued by Manulife (Singapore) Pte. Ltd.,
against LIA MU 20/15 and FAA-N16, with personal identifiers redacted. Raw source PDFs remain in the local workspace only and are never committed to this
repo.
