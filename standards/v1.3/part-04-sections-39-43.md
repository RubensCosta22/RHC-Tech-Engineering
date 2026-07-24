# 39. REPOSITORY & DOCUMENTATION STRUCTURE

Every active RHC Tech repository should adopt a predictable engineering documentation structure as the Legacy Adoption triggers in Section 34 are reached.

Recommended baseline:

```text
/docs
  /product
    product-spec.md

  /architecture
    architecture.md

  /domains
    authentication.md
    authorization.md
    observability.md

  /specs
    /<SPEC-ID>
      spec.md
      implementation-plan.md
      verification.md

  /adr
    ADR-001.md

  /security
    security-standard.md
```

Repositories may adapt the structure when justified by size or technology, but the location of active Specs, ADRs and verification evidence must be obvious.

Each Feature Spec directory should normally contain:

### `spec.md`

Approved behavior and requirements.

### `implementation-plan.md`

Approved technical execution plan.

### `verification.md`

Acceptance results, test evidence, production verification and residual risks.

Repository automation should progressively enforce this standard through pull-request templates, CI quality gates, protected branches and required checks, prioritizing controls that correspond to the repository's active R2–R4 risks.

---

# 40. STANDARD VS OPERATIONAL PLAYBOOK

RHC Tech separates normative engineering rules from daily execution guidance.

## 40.1 SDD Standard

This document is the **normative standard**.

It defines what is mandatory, what blocks approval or release, the risk model, governance rules and engineering principles.

Statements using terms such as `must`, `required`, `cannot`, `blocks` or equivalent normative language establish obligations.

## 40.2 Operational Playbook

The Operational Playbook explains how to satisfy this standard efficiently.

It may contain:

* step-by-step workflows;
* examples;
* prompts for AI agents;
* review procedures;
* GitHub workflows;
* migration runbooks;
* verification procedures;
* common implementation patterns.

## 40.3 Templates

Templates provide reusable artifacts such as:

* Feature Spec;
* R1 Lite Spec;
* Bug Spec;
* Implementation Plan;
* ADR;
* Verification Report;
* Exception Record.

## 40.4 Risk Checklists

Daily execution should primarily use tier-specific checklists derived from Section 32:

```text
R0 Checklist
R1 Checklist
R2 Checklist
R3 Checklist
R4 Checklist
```

The checklist must make the applicable controls obvious without requiring the implementer to reread the full Standard for every change.

## 40.5 Conflict Rule

If a Playbook, template, checklist, AI prompt or repository convention conflicts with this Standard, **the Standard wins**.

The Playbook and templates may evolve without a new Standard version when the change only improves execution guidance and does not add, remove or weaken a normative requirement.

A change to mandatory engineering obligations requires a new version of the Standard.

---

# 41. ADVERSARIAL REVIEW CHECKPOINT

Before any Spec that requires approval enters `Approved`, the reviewer must ask:

> What would make this Spec unsafe, ambiguous, incomplete, untestable or likely to cause a regression if implemented exactly as written?

Approval is not a ceremony.

Approval means the Spec has survived a serious attempt to find reasons it should **not** be implemented yet.

For solo development, this checkpoint is fulfilled through the mandatory adversarial AI review defined in Section 29 for R1–R4 changes.

R0 lightweight records do not require adversarial review unless the change is reclassified or a conditional control reveals material risk.

---

# 42. VERSION TRANSITION NOTE

RHC Tech SDD v1.3 supersedes v1.2 because the corrections below change normative obligations and therefore require a new Standard version under Section 40:

* PR references now follow the engineering record required by Risk Tier rather than requiring a Feature Spec universally;
* Definition of Done is explicitly governed by the Section 32 applicability matrix;
* the standard pipeline now branches by Risk Tier and no longer imposes R1–R4 review steps on R0;
* production verification is required only when Section 32 makes it Required or Conditional-and-applicable;
* R3/R4 adversarial review independence is strengthened when tooling supports separate contexts.

Existing v1.2 records remain historical evidence and do not need rewriting. New or materially revised work uses v1.3 after adoption.

---

# 43. RHC TECH SDD v1.3 ENFORCEMENT RULE

The standard is effective only when it changes engineering behavior without creating unnecessary process burden.

Therefore:

**No Risk Tier → No Ready.**

**No Applicable Controls → No Ready.**

**No Ready → No Code.**

**No required independent review → No Approved.**

**No evidence → No Done.**

**No passed mandatory gates → No Release.**

When development is solo, required independent review for R1–R4 means the mandatory adversarial AI review plus explicit human-owner approval.

The SDD Process Budget may simplify documentation and workflow, but it may never remove a mandatory control from the applicable risk tier.

RHC Tech optimizes for:

> **Specified. Reviewed. Implemented. Verified. Reliable.**
