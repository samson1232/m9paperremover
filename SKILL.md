---
name: m9paperremover
description: "Audit insurance docs for KYC flaws; draft complaints."
version: 1.0
category: compliance
license: MIT
metadata:
  hermes:
    tags: [audit, compliance, kyc, document-analysis, complaint]
    related_skills: [pdf, docx]
author: m9paperremover-skill
---

## When to Use

Use when you need to audit a life-insurance policy document (with its supporting application, facts report, and product illustration) against the LIA Members' Undertaking MU 20/15 (Minimum Standard for Life Insurance Advisory Process) and FAA-N16, to surface KYC, suitability, or compliance findings and draft a formal complaint requesting reversal of a transaction. Examples: a reviewer working a policy/application file for KYC integrity or mis-selling concerns; a compliance officer mapping a complaint to specific MU 20/15 clauses; a preparer building a reversal request with channel routing.

# M9PaperRemover — KYC / advisory-document audit

## 1. What this does

Audits a life-insurance policy document (plus its application, Plan Right-style Fact Find, and Product Illustration) against FAA-N16 and LIA Members' Undertaking No. 20 of 2015 (MU 20/15, Minimum Standard for Life Insurance Advisory Process), spotting KYC / suitability / compliance findings, then drafts a formal complaint requesting full reversal and gives routing/channel guidance.

## 2. Inputs

1. Policy document (PDF) — contract, schedule pages, charges/MIP tables, appendix rate tables.
2. Application / Guaranteed Issuance Offer — personal-data KYC sections (A-G), residency declaration, authentication signature block, Myinfo retrieval date.
3. Facts report (Plan Right / Policy Illustration, Product Summary).
4. LIA MU 20/15 PDF (for clause cross-referencing).
5. Transaction / statement of account (premiums paid, fund allocation).

## 3. Procedure

### Step 1 — Extract and normalize
- Pull text per page (pymupdf/fitz or pdf_read.py).
- Capture: Policy No., Owner/Life Insured, Rep name + agent code + firm/agency, issue date, Myinfo retrieval date, annual premium, MIP, basic premium allocation, fund allocation, charges schedule.
- Note date inconsistencies as potential red flags: Myinfo retrieval vs application date vs issue date; generated date vs signed date; signature-date formats.

### Step 2 — KYC cross-reference (MU 20/15 §3.9 "Know your client")
For each requirement, check against the record and evidence:
- §3.9(a) Personal information: name, NRIC/passport, citizenship, country of birth, gender, DOB, marital status, residential/mailing address, ID type.
- §3.9(b) Priorities/objectives: goals, shortfalls, time horizon.
- §3.9(c) Investment profile: risk profile, CKA/CAR outcome and justifications.
- §3.9(d) Cash flow & budget: income, expenditure, net cash flow, affordability vs premium.
- §3.9(e) Assets & liabilities: net worth, mortgages, properties, residency.
- §3.9(f) Existing insurance portfolio: policies, status, coverage.

### Step 3 — Needs analysis (MU 20/15 §3.10)
- §3.10(a) Protection needs (dependants/legacy).
- §3.10(b) Savings & investment needs (shortfall / why this product).
- §3.10(c) A&H needs (existing coverage gaps).

### Step 4 — Recommendation (MU 20/15 §3.11)
- §3.11(c) Reasons for recommendations — basis documented?
- §3.11(d) Risks and limitations of the recommended plan — flagged?
- §3.11(e) Reasons for deviation — if product deviates from client profile/cashflow, documented?

### Step 5 — Suitability & KYC integrity flags
Score each finding MVI (Major) / V (High) / LVI (Low). Typical MVI flags:


- Mandatory advisory-form fields blank/N/A (dependants, spouse, children) but policy issued.
- CKA/CAR outcome not justified by cashflow or supporting docs.
- Premium affordability > 30% (or the report's own threshold) of take-home income, with other cashflow strain (mortgages, other properties), never addressed; source-of-fund not documented.
- Authentication/identity verification marked without documented ID/proof-of-address; signature-date formatting defects.

### Step 6 — Map to MU 20/15 clause and build the complaint
- Each MVI: Claim (finding) + Evidence (document extract) + Regulatory basis (MU 20/15 §x.y, FAA-N16, COLIP, Insurance Act s.23(5)).
- Requested relief: full review; refund of premiums without interest; rescission/reversal; explanation; referral.
- Route: insurer complaint-handling → rep/FA firm channel → LIA (standards body, no dispute authority) → MAS/PDPC (regulatory) → FIDReC (dispute) → Police (criminal). See references/delivery-matrix.md.

### Step 7 — Verify
- Re-read the complaint against the source PDF: every claim traceable to an extract.
- Redact real personal identifiers (names, NRIC, full address, phone/email, signature) before sharing or persisting.

## 4. Common pitfalls

- Garbled PDF text (e.g. "s05-Sep-2026", "26 08 n io rs Ve") — treat as OCR/extraction artifacts; verify against the original visual page before claiming a defect.
- Blank/redacted fields in extracted text (e.g. policy No. field) — confirm whether truly blank or hidden; do not assert a defect from extraction alone.
- Myinfo retrieval vs issue date — flag the inconsistency, but do not over-claim the direction of causation without evidence.
- Map to MU 20/15 paragraph numbers only where the PDF explicitly uses them (advisory form sections §3.8–§3.15).
- PII hygiene: redact names, NRIC, addresses, contacts, signatures before persisting or sharing.

## 5. Deliverable checklist

- [ ] Findings table: Finding | MU 20/15 clause | Evidence | Severity
- [ ] Recommendation-suitability summary (note the 50%-affordability threshold)
- [ ] Complaint draft: purpose, background, findings, requested relief, support docs, delivery matrix
- [ ] Routing/channel list with timings
- [ ] PII redaction log

## 6. Reference files

- references/delivery-matrix.md (channels, contacts, timings, flags)
- references/analysis-template.md (findings / claim / evidence / regulatory-basis table)
- references/complaint-template.md (fill-in complaint draft with PII placeholders)

## 6.1 Redacted findings example

The following is the output shape of this skill applied to a single policy, with the real
personal identifiers redacted. Reference the local workspace PDFs for the original
extracts. Findings are cross-referenced to the LIA MU 20/15 advisory-form sections used
by the source document.

| # | Finding (redacted) | Severity | MU 20/15 clause | Evidence (redacted) | Action |
|---|---|---|---|---|---|
| 1 | Needs analysis incomplete — no dependant / spouse / child information documented; three sons' insurable-needs not assessed | MVI | §3.9(a), §3.10(a)–(b) | Plan Right report records "No dependant information provided", "No child information provided" | complaint §5.1 |
| 2 | CKA "PASSED CKA" not grounded in cashflow — multiple mortgages contradict the "Growth"/S$35,000-premium premise | MVI | §3.9(c), (d), (e) | Plan Right report "PASSED CKA"; mortgage commitments; annual premium S$35,000 vs take-home ~56% | complaint §5.1 |
| 3 | Affordability not addressed — premium > 30% of wage; houses + mortgage strain incl. Malaysia property RM1.1m and RM400k; source of fund undocumented | MVI | §3.9(d), (e); §3.10(b); §3.11(c) | Plan Right report "Annualized budget exceeds 30% of cashflow"; properties affecting cashflow | complaint §5.1 |
| 4 | Authentication/identity verification not evidenced; signature dates garbled | V | Application authentication block | §3.9(a), §4.25 | complaint §5.1 |

## 6.2 Withdrawn points (clarified by the Complainant; not relied on in the complaint)


These were withdrawn at the Complainant's instruction and are marked [REMOVED] in the
stored complaint draft; they are shown here only as worked examples of the "withdrawn"
annotation format.

## 7. Provenance notes

Keep the raw source PDFs (which contain the original personal identifiers) in the local workspace, referenced by path, and NEVER in this skill. This skill stores the methodology and a redacted analysis example only.
