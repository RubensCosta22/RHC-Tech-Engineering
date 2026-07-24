# 28. EFFECTIVE RULE

Starting with RHC Tech SDD v1.3:

**No relevant new feature, redesign, architectural modification, database change or security-sensitive change should enter development without the specification and controls required by its risk tier.**

Existing products are adopted through the explicit Legacy Adoption & Touch Rule in Section 34. Untouched legacy code is not required to be retrofitted solely to satisfy this standard, but newly changed surfaces must follow the current applicable controls.

RHC Tech is no longer optimizing for:

> “It works.”

The engineering standard becomes:

> **Specified. Reviewed. Implemented. Verified. Reliable.**

---

# 29. GOVERNANCE & APPROVAL AUTHORITY

RHC Tech separates **ownership**, **review** and **approval authority** even when the same person performs more than one role.

Relevant roles are:

### Product Owner

Approves problem definition, product behavior, business scope and non-goals.

### Engineering Owner

Approves architecture, implementation boundaries, technical risk and migration strategy.

### Security Reviewer

Required for security-sensitive changes, including authentication, authorization, RLS, secrets, sensitive data, uploads, privilege boundaries and externally exposed interfaces.

### Design / Brand Reviewer

Required for material user-facing changes. Verifies UX states, accessibility, responsive behavior and RHC Tech Brand System compliance.

### Release Owner

Owns the decision to release and verifies that blocking gates are satisfied.

One person may hold multiple roles in a small team, but the responsibilities must remain explicit.

## Solo Development Rule — Mandatory Adversarial AI Review

When R1–R4 work is being developed by a single human owner without an independent human reviewer, an **AI agent must act as a mandatory adversarial reviewer before the Spec can move from `In Review` to `Approved`.** R0 lightweight records do not require this review unless the change is reclassified or an applicable conditional control reveals material risk.

The adversarial reviewer must attempt to disprove the readiness of the Spec rather than merely confirm it.

It must actively search for:

* ambiguous or contradictory requirements;
* missing acceptance criteria;
* hidden scope expansion;
* authorization and privilege-escalation flaws;
* privacy and sensitive-data exposure;
* migration and rollback risk;
* silent failure paths;
* missing observability;
* unhandled UX states;
* accessibility gaps;
* performance or reliability risks;
* missing tests;
* regressions against existing behavior;
* conflicts with architecture or the Brand System.

The reviewer must return findings classified at minimum as:

`Blocker`, `Major`, `Minor`, `Accepted Risk`.

A Spec with unresolved `Blocker` findings cannot become `Approved`.

For R3 and R4 changes, unresolved `Major` findings also block approval unless explicitly recorded as an approved exception under Section 24.

The AI reviewer does **not** replace human accountability. The human owner remains responsible for the approval decision, but solo development may not bypass independent adversarial review merely because no second human engineer is available.

For R1 and R2, the AI adversarial review should be performed in a separate review pass or agent context whenever practical.

For R3 and R4, the adversarial AI review **must use a separate review context from the authoring or implementation pass whenever the available tooling supports separation**. If tooling cannot provide a separate context, the limitation must be recorded explicitly in the review and treated as a review-independence risk.

Every R3/R4 adversarial review must record, at minimum:

* Spec ID and version;
* reviewed artifact version, commit or stable content reference;
* review date;
* reviewer identity/context;
* whether the review context was independent from authoring/implementation.

## Exception approval

Exceptions under Section 24 require approval from the role accountable for the bypassed rule.

Security exceptions require Security Reviewer approval. Release-blocking exceptions require Release Owner approval. In solo development, the exception must also be included in the adversarial AI review before release.

---

# 30. SPEC IDENTIFICATION & VERSIONING

Every formal Spec must have a stable identifier.

Recommended format:

```text
<PRODUCT>-<DOMAIN>-<NUMBER>
```

Examples:

```text
RHCT-AUTH-001
RHCT-WORKOUT-003
HNT-FINANCE-002
INSP-REPORT-001
```

Each Spec must include:

* Spec ID;
* version;
* status;
* created date;
* last updated date;
* owner;
* required approvers;
* change risk classification;
* related ADRs;
* related PRs;
* target or actual release;
* superseded Spec, when applicable.

Approved requirements must not be rewritten silently. Material changes after approval require a version change and renewed review.

Historical versions must remain traceable.

---

# 31. CHANGE RISK CLASSIFICATION

Every relevant change must receive a risk classification before implementation.

**Risk tier measures potential impact and control requirements. It does not estimate implementation effort.**

A low-risk change may still require substantial implementation time. A high-risk change may be technically small. Risk classification must never be lowered merely because the work appears easy.

## R0 — Trivial

Documentation, copy or maintenance with no runtime behavior change.

May use a lightweight change record instead of a full Feature Spec.

## R1 — Low

Isolated behavior with limited impact, no sensitive authorization boundary and low regression risk.

Uses a lightweight Feature Spec or equivalent bounded record with acceptance criteria and the applicable R1 controls.

## R2 — Medium

Meaningful business logic, API, database, workflow or user-facing behavior change.

Requires normal Feature Spec, automated verification appropriate to the change and manual validation when user-facing.

## R3 — High

Authentication, authorization, RLS, personal or sensitive data, critical migrations, destructive operations, external security boundaries, critical infrastructure or high-impact business behavior.

Requires explicit security review, rollback/remediation strategy, permission testing, stronger evidence and post-release verification.

## R4 — Critical

A change capable of causing broad data exposure, irreversible data loss, major security compromise, systemic outage or equivalent impact.

Requires the strictest available review, staged rollout where technically possible, explicit release approval and documented recovery procedure.

Risk may be raised at any point during review or implementation. It may not be lowered merely to avoid required gates.

---

# 32. RISK TIER APPLICABILITY MATRIX

The risk tier is the operational index for this standard.

A developer or AI agent should not need to reread the entire SDD to determine the normal control set for a routine change.

Legend:

* `R` — Required.
* `C` — Conditional. Required when the change affects that surface or introduces that risk.
* `Lite` — Required in simplified form.
* `—` — Not normally required.

| Control | R0 | R1 | R2 | R3 | R4 |
|---|:---:|:---:|:---:|:---:|:---:|
| Change record / Feature Spec | Lite | Lite | R | R | R |
| Acceptance Criteria | C | R | R | R | R |
| Solo AI Adversarial Review | — | R | R | R | R |
| Architecture Review | — | — | C | R | R |
| ADR | — | — | C | C | R |
| Security Review | — | C | C | R | R |
| Authorization / RLS Review | — | C | C | R | R |
| Privacy / LGPD Review | — | C | C | R | R |
| Database Impact Review | — | C | C | R | R |
| Migration Rollback / Remediation | — | — | C | R | R |
| Observability / Failure Handling | — | C | R | R | R |
| Performance Budget | — | — | C | R | R |
| Reliability Review | — | — | C | R | R |
| Supply Chain Review | — | C | C | R | R |
| Traceability | — | Lite | R | R | R |
| Automated Regression Testing | C | C | R | R | R |
| Manual UX Validation | — | C | C | R | R |
| Production Verification | — | C | R | R | R |
| Explicit Risk Acceptance | — | — | C | R | R |

`Conditional` does not mean optional by preference. It means the control becomes mandatory when the relevant surface is affected.

Examples:

* an R1 copy change does not require an RLS review;
* an R1 dependency update requires the applicable supply-chain checks;
* an R2 feature touching authorization requires authorization testing even though authorization review is marked `C` for R2;
* any change involving destructive migration behavior must apply the database and remediation controls appropriate to the actual risk, and the risk tier should be raised when necessary.

Every formal Spec must include an **Applicable Controls** block derived from this matrix.

Recommended format:

```text
Risk Tier: R2

Applicable Controls:
[x] Feature Spec
[x] Acceptance Criteria
[x] Solo AI Adversarial Review
[x] Observability
[x] Regression Tests
[ ] Privacy
[ ] Database Migration
[ ] ADR
[ ] Performance Budget
```

An AI agent implementing an approved Spec must treat the declared control set as part of the specification contract.

---

# 33. SDD PROCESS BUDGET

RHC Tech applies rigor according to risk while actively preventing process overhead from becoming larger than the value of the change.

The SDD Process Budget controls the expected time spent on **specification, review and approval overhead**. It does not limit implementation, debugging, CI execution, deployment or production observation time.

**Risk tier determines rigor, not estimated implementation time.**

Recommended targets for solo or small-team development:

| Risk Tier | Target SDD Process Budget |
|---|---|
| R0 | Up to 15 minutes for a lightweight change record/checklist |
| R1 | Up to 45 minutes from change definition through approval |
| R2 | Up to one focused work session from Spec drafting through approval |
| R3 | Up to two focused work sessions for Spec, required reviews and approval |
| R4 | No artificial time cap; risk control takes priority |

A **focused work session** means one normal uninterrupted engineering work block chosen by the owner. It is intentionally session-based rather than tied to a fixed calendar duration so that the standard remains usable alongside employment, study and other commitments.

These are process targets, not deadlines and not SLAs.

Exceeding a budget is not automatically a failure. It is a signal to ask whether:

* the Spec is unnecessarily verbose;
* the change contains more than one concern and should be split;
* the risk tier is understated;
* the template or checklist can be simplified;
* unresolved product decisions are being disguised as engineering work.

**If the SDD process becomes materially more expensive than the change itself, the process must be simplified without removing mandatory risk controls.**

A time budget must never be used to bypass security, authorization, privacy, migration safety, regression protection or release-blocking evidence.

---

# 34. LEGACY ADOPTION & TOUCH RULE

Existing RHC Tech products do not require a full retrospective rewrite before useful development can continue.

Adoption is **touch-driven and risk-driven**, not based on an arbitrary calendar deadline.

## 34.1 Untouched Legacy Code

Untouched legacy code may remain as-is.

Legacy code does not need to be refactored or documented solely because RHC Tech adopted a newer SDD version.

## 34.2 Material Touch Rule

When an existing surface is materially modified:

* the new or changed behavior must follow the current SDD version;
* affected requirements and acceptance criteria must be documented at the level required by the risk tier;
* applicable controls from Section 32 must be executed;
* newly discovered material legacy risks must be recorded;
* unrelated legacy debt does not automatically enter the scope of the change.

A **material touch** includes a change to runtime behavior, business logic, data access, persistence, permissions, architecture, integration behavior, user journey, critical observability or security posture.

Pure formatting, comments, documentation or mechanical changes with no runtime effect are not material touches.

## 34.3 No Grandfathering for Critical Surfaces

The following areas do not receive a legacy exemption when changed or when a meaningful defect is discovered:

* S0 or S1 defects;
* authentication;
* authorization and RLS;
* cross-account or cross-profile data exposure;
* personal or sensitive data handling;
* secrets or credential handling;
* destructive or high-risk migrations;
* irreversible data-loss paths;
* security-sensitive file access or uploads.

These must conform immediately to the current applicable SDD controls before the change is released.

## 34.4 Domain Baseline Trigger

A legacy domain must receive or update its L1 Domain/System Spec before any of the following occurs:

* the domain receives an R3 or R4 change;
* an architectural boundary in the domain changes;
* authentication, authorization or ownership rules are materially changed;
* the domain reaches its third R2-or-higher material change under the current SDD without a current L1 baseline;
* the product is prepared for commercialization, external beta or a major release where the domain is part of a critical journey.

This rule allows documentation to grow with the code that is actually being changed instead of forcing a complete historical rewrite.

## 34.5 Product Baseline Trigger

An existing product must have a current L0 Product Spec and baseline architecture/security sources of truth before:

* its first R3 or R4 change under this standard;
* commercialization or external beta;
* a major release intended to establish a new production baseline.

There is no requirement to pause a stable personal product merely to backfill historical documentation when none of these triggers has occurred.

---

# 35. PRIVACY & DATA GOVERNANCE

Privacy is a product and engineering requirement, not only a legal review activity.

Every Spec that creates, reads, transforms, transmits or deletes personal data must document, when applicable:

* what data is collected;
* why it is necessary;
* legal or product purpose;
* data owner or subject;
* who can access it;
* where it is stored;
* whether it is transmitted to third parties;
* retention expectations;
* deletion behavior;
* export or portability behavior;
* backup implications;
* logging implications;
* whether the data is sensitive;
* masking, minimization or pseudonymization requirements.

RHC Tech products should follow data minimization: do not collect or retain personal data without a defined purpose.

Brazilian products and users must be evaluated for applicable LGPD obligations. Other jurisdictions must be evaluated when the product scope requires them.

Deletion must consider primary storage, derived data, cached data and backups according to the applicable product policy.

Applicability is determined by Section 32. A lower-risk change becomes subject to this section whenever it touches personal or sensitive data.

---

# 36. PERFORMANCE & RELIABILITY REQUIREMENTS

Specs must define measurable performance or reliability expectations when degradation could materially affect the user or system.

Relevant performance considerations include:

* page or interaction responsiveness;
* API latency;
* database query cost and latency;
* bundle-size impact;
* image and media limits;
* background-job duration;
* acceptable regression from current performance.

Relevant reliability considerations include:

* timeout policy;
* retry policy;
* idempotency;
* rate limiting;
* concurrency behavior;
* transaction boundaries;
* degraded mode;
* recovery behavior;
* availability expectations for critical journeys.

Performance budgets should be objective whenever technically reasonable.

A feature that is functionally correct but introduces an unacceptable material performance or reliability regression is not Done.

Applicability is determined by Section 32. R3 and R4 changes require explicit consideration; R2 changes require it when the affected surface can materially degrade performance or reliability.

---

# 37. SOFTWARE SUPPLY CHAIN SECURITY

Third-party code is part of the RHC Tech attack surface.

According to Section 32 and repository capabilities, required checks may include:

* dependency vulnerability scanning;
* secret scanning;
* dependency review;
* lockfile integrity;
* unsupported or abandoned critical dependency review;
* critical/high CVE evaluation;
* build provenance or artifact verification where applicable;
* SBOM generation for products or releases where justified by risk or commercialization requirements.

A dependency must not be added merely because it simplifies implementation.

Before introducing a meaningful dependency, evaluate:

* necessity;
* maintenance health;
* security history;
* license compatibility;
* bundle or runtime impact;
* transitive dependency cost;
* replacement difficulty.

Known exploitable critical vulnerabilities block release unless an explicit exception is approved.

---

# 38. TRACEABILITY & EVIDENCE MATRIX

Material requirements must be traceable from specification to verification according to Section 32.

For each relevant Functional Requirement or Acceptance Criterion, the project should be able to identify, when applicable:

```text
Requirement → Implementation → Test → Evidence → Release
```

Recommended verification matrix:

| Requirement | Implementation | Test | Evidence | Status |
|---|---|---|---|---|
| FR-01 | module/file | automated/manual test | CI, screenshot or query | Pass/Fail |
| AC-01 | route/policy/component | E2E or integration test | test output | Pass/Fail |

For R1, traceability may remain lightweight and be maintained directly in the PR or change record.

For R2, R3 and R4, material Functional Requirements and Acceptance Criteria must be traceable to verification evidence.

For R3 and R4 changes, traceability is mandatory for all security-, permission-, data-integrity- and release-critical requirements.

The goal is not paperwork.

The goal is to answer objectively:

> Where is the evidence that this requirement works?

---
