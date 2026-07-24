# RHC Tech — SDD Operational Playbook

**Standard baseline:** RHC Tech SDD v1.3  
**Purpose:** explain how to execute the Standard efficiently without changing its normative obligations.

If this playbook conflicts with the Standard, **the Standard wins**.

## 1. Start every relevant change

1. Define the problem or change.
2. Assign Risk Tier R0–R4 based on potential impact, not effort.
3. Derive Applicable Controls from `checklists/risk-tiers.md`.
4. Select the engineering record required by the tier.
5. Assess legacy-touch and baseline triggers.

### R0

Use `templates/lightweight-change-record.md`.

R0 is valid only when runtime behavior is provably unchanged. Reclassify immediately if material impact appears.

### R1

Use `templates/r1-lite-spec.md`.

Acceptance Criteria and solo adversarial review are mandatory. Conditional controls become mandatory when their surface is touched.

### R2–R4

Use `templates/feature-spec.md`, then the required adversarial review and `templates/implementation-plan.md` before implementation.

## 2. Solo adversarial review

For R1–R4, use `templates/adversarial-review.md` and explicitly challenge the Spec rather than confirming it.

The reviewer should answer:

> What would make this Spec unsafe, ambiguous, incomplete, untestable or likely to cause a regression if implemented exactly as written?

For R3/R4, use a separate review context whenever tooling supports it and record the reviewed artifact/commit/reference.

## 3. Implementation

Implement only after the item is Ready and Approved where approval is required.

Implementation may decide low-level details inside the Spec boundaries. It may not silently change business rules, validation, permissions, security, UX flows, persistence or scope.

A material discovery returns the record to review.

## 4. Verification

Use `templates/verification.md` for R2–R4 and whenever a lower tier needs formal evidence.

Map material requirements to:

```text
Requirement → Implementation → Test → Evidence → Release
```

Do not mark a gate N/A without a reason when the control is conditional.

## 5. Pull requests

Every relevant PR references the engineering record required by its tier and states:

- Risk Tier;
- Applicable Controls;
- what changed and did not change;
- security/data/UX impact;
- tests and evidence;
- residual risks/exceptions;
- release/production verification when applicable.

Prefer cohesive PRs over excessive fragmentation, but never combine unrelated features only to reduce PR count.

## 6. Bugs

Use `templates/bug-record.md` for meaningful defects.

Record expected behavior, actual behavior, reproduction, root cause, impact, fix strategy, regression risk and verification. Symptom-only patches are insufficient for meaningful bugs.

## 7. Architecture decisions

Use `templates/adr.md` when Section 32 requires or when a durable architecture choice needs explicit rationale and tradeoffs.

## 8. Exceptions

Use `templates/exception-record.md`.

Exceptions are explicit, risk-assessed, approved by the accountable role and time/condition bounded. They never silently weaken the Standard.

## 9. Legacy products

Adoption is touch-driven and risk-driven.

Do not stop a stable product only to document untouched legacy code. When a surface is materially changed, apply current controls to the changed behavior and record newly discovered material risks.

Critical auth, RLS, cross-account exposure, sensitive-data, secret, destructive-migration and data-loss surfaces receive no grandfathering when touched or when a meaningful defect is discovered.

## 10. Release rule

Runtime production verification is performed only when required or conditional-and-applicable under the Risk Tier matrix.

A successful deploy is not proof that an applicable runtime change is healthy.

## 11. Enforcement shorthand

**No Risk Tier → No Ready.**  
**No Applicable Controls → No Ready.**  
**No Ready → No Code.**  
**No required independent review → No Approved.**  
**No evidence → No Done.**  
**No passed mandatory gates → No Release.**

> **Specified. Reviewed. Implemented. Verified. Reliable.**
