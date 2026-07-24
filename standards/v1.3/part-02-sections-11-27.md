# 11. DATA & DATABASE CHANGES

Database changes require special treatment.

Every migration must define:

### Objective

### SQL migration

### Compatibility

### Existing-data impact

### RLS impact

### Rollback or remediation strategy

### Validation query

### Post-migration verification

A migration is not considered complete simply because Supabase accepted the SQL.

Its effect must be verified.

Destructive migrations require explicit review.

---

# 12. TEST STRATEGY

Tests are selected based on risk, not as a checkbox.

The implementation plan must identify which levels are required.

### Unit tests

For isolated business logic.

### Integration tests

For database, APIs, authentication and service boundaries.

### Permission tests

Mandatory for authorization-sensitive functionality.

### Regression tests

Mandatory when fixing a previous defect.

### End-to-end tests

For critical user journeys.

### Manual UX validation

For visual, responsive and interaction-sensitive changes.

A bug that reaches production should normally result in a regression test when technically reasonable.

---

# 13. ACCEPTANCE CRITERIA

Every Feature Spec must finish with objective acceptance criteria.

Use Given / When / Then when useful.

Example:

**AC-01**

Given an authenticated Henrique account
When the training profile selector is loaded
Then only profiles authorized for that account may be displayed.

**AC-02**

Given an unauthorized profile identifier
When the client attempts to load that profile directly
Then access must be rejected by the data layer regardless of frontend behavior.

Acceptance criteria describe observable behavior.

They do not say:

> Code looks good.

They say:

> The system behaves correctly under defined conditions.

---

# 14. DEFINITION OF READY

Development cannot begin until the item is **Ready**.

A Spec is Ready only when:

* problem is clear;
* scope is clear;
* non-goals are documented;
* functional requirements exist;
* business rules are resolved;
* authorization rules are defined;
* UX states are defined when applicable;
* architecture impact is understood;
* security risks were considered;
* privacy and data-governance impact was considered when applicable;
* change risk classification is assigned;
* applicable controls are declared using the Risk Tier Applicability Matrix;
* the SDD process budget target is identified for R0–R3 work;
* acceptance criteria exist;
* unresolved questions capable of changing implementation are closed.

No Ready → No Code.

---

# 15. IMPLEMENTATION

Implementation must follow the approved Spec.

Developers and AI agents may make low-level technical decisions within the approved boundaries.

They may not silently:

* change business rules;
* remove validation;
* weaken security;
* alter permissions;
* redesign flows;
* expand scope;
* change persistence behavior.

A material requirement change returns the feature to:

**Spec Review**

---

# 16. AI DEVELOPMENT POLICY

AI is an engineering tool.

It is not an authority over the specification.

Any AI agent working on RHC Tech code must receive:

1. the approved Spec;
2. relevant architecture context;
3. relevant Brand System rules;
4. affected code context;
5. constraints;
6. acceptance criteria;
7. verification requirements.

Prompts such as:

> Improve this page.

or

> Make it professional.

are insufficient for production development.

The preferred format is:

> Implement SPEC-XXX exactly within the following constraints.

AI-generated code receives the same quality requirements as human-written code.

When AI is used as the mandatory adversarial reviewer in solo development, the review must be explicitly framed as an independent challenge to the Spec. The review context should not assume that previous AI-generated decisions are correct merely because they were generated earlier.

---

# 17. CHANGE SIZE

RHC Tech prefers meaningful, cohesive PRs instead of excessive fragmentation.

However:

**Large PR does not mean undefined PR.**

A larger PR must still have:

* bounded scope;
* approved Spec;
* implementation plan;
* clear commit/section organization;
* verifiable acceptance criteria.

Multiple unrelated features should not be combined only to reduce the number of PRs.

---

# 18. PULL REQUEST STANDARD

Every relevant PR must reference the approved engineering record required by its Risk Tier: a Feature Spec, R1 Lite Spec or R0 Lightweight Change Record. Non-relevant administrative PRs may omit a formal engineering record when they provably fall outside the Relevant Change definition.

Recommended structure:

## Engineering Record

`<SPEC-ID or CHANGE-ID>`

The PR must state the Risk Tier and Applicable Controls. For R0, the lightweight record is sufficient unless reclassification occurs.

## What changed

Implementation summary.

## What did not change

Important preserved behavior.

## Database impact

Migrations and data implications.

## Security impact

Authorization, validation and data exposure.

## UX impact

Screens and states affected.

## Tests

Automated and manual verification performed.

## Traceability

Map relevant requirements and acceptance criteria to tests and evidence.

## Evidence

Where useful:

* screenshots;
* test output;
* build output;
* database verification;
* before/after behavior.

## Risks

Known residual risk.

---

# 19. QUALITY GATES

A PR cannot be considered release-ready while required gates fail.

Which gates are mandatory for a change is determined first by the Risk Tier Applicability Matrix in Section 32 and then by the actual surfaces affected by the change. A control marked `Conditional` becomes mandatory when that surface is affected.

Typical quality gates:

**G1 — Spec Compliance**

Implementation matches approved requirements.

**G2 — Build**

Production build succeeds.

**G3 — Static Quality**

Lint/type checks succeed when available.

**G4 — Tests**

Required automated tests succeed.

**G5 — Security**

No known critical/high security issue introduced.

**G6 — Permissions**

Authorization behavior verified.

**G7 — UX**

Required states and responsive layouts verified.

**G8 — Brand**

User-facing changes comply with RHC Tech Brand System.

**G9 — Observability**

Critical new flows have appropriate failure handling and telemetry.

**G10 — Privacy & Data Governance**

Applicable personal-data, retention, deletion, export and third-party exposure requirements are verified.

**G11 — Supply Chain Security**

Required dependency, secret and vulnerability checks pass according to change risk.

**G12 — Performance & Reliability**

Applicable performance budgets, timeout, retry, idempotency and degraded-mode requirements are verified.

**G13 — Regression**

Existing critical flows remain operational.

Failure of a mandatory gate blocks release.

---

# 20. DEFINITION OF DONE

A change is Done only when all controls required by its Risk Tier and all Conditional controls activated by the affected surfaces have been satisfied. Section 32 is authoritative for applicability.

When applicable, Done requires that:

* the approved engineering record was implemented as specified;
* required acceptance criteria pass;
* required tests pass;
* build/static checks pass when applicable to the repository and change;
* authorization was verified when authorization is affected;
* migrations were validated when database changes exist;
* errors are handled intentionally when runtime failure paths are affected;
* logs do not expose sensitive information when logging/telemetry is involved;
* responsive behavior, accessibility and Brand System compliance were checked for material user-facing changes;
* privacy requirements were verified when personal or sensitive data is affected;
* supply-chain security checks passed when dependencies or build supply chain are affected;
* performance and reliability requirements were verified when applicable;
* traceability evidence exists at the depth required by Section 32;
* no known blocking regression remains;
* documentation was updated when required;
* release and production evidence exists when required by the Risk Tier.

An item marked `—` in Section 32 is not required merely to satisfy this section. A `Conditional` item becomes mandatory only when its relevant surface or risk is affected.

**Merged does not automatically mean Done.**

---

# 21. RELEASE STANDARD

This section applies when a change produces a runtime release or when Production Verification is Required or becomes Conditional-and-applicable under Section 32. Documentation-only or process-only R0 changes do not require runtime production verification.

Applicable releases must be treated as engineering events.

Before production release, when relevant:

### Pre-release

* verify target commit;
* verify environment configuration;
* verify migrations when present;
* verify secrets/configuration when affected;
* run all mandatory gates derived from Section 32;
* verify rollback/remediation strategy when required by risk or affected surface.

### Post-release

When Production Verification applies, verify the critical affected journey in production and only the surfaces relevant to the change, such as:

* authentication;
* API/data access;
* critical UI;
* storage;
* telemetry;
* major errors.

Do not require unrelated production checks merely because they appear in this list.

A deployment that completed successfully is not proof that an applicable runtime change is healthy.

---

# 22. BUG POLICY

Bug fixing must also be Spec-Driven.

A bug record must contain:

### Expected behavior

### Actual behavior

### Reproduction

### Root cause

### Impact

### Fix strategy

### Regression risk

### Verification

For meaningful bugs, the root cause matters.

Patching only the visible symptom is insufficient.

---

# 23. SEVERITY

RHC Tech uses four primary defect levels.

### S0 — Critical

Security breach, irreversible data loss, total system outage or equivalent impact.

Blocks release immediately.

### S1 — High

Core functionality broken, serious authorization failure or major production regression.

Blocks release.

### S2 — Medium

Important defect with workaround or limited impact.

Normally fixed before planned release when inside affected scope.

### S3 — Low

Minor defect, polish issue or low-impact inconsistency.

May be scheduled later.

---

# 24. EXCEPTIONS

A standard may occasionally require an exception.

Exceptions must be explicit.

Never silent.

An exception record must contain:

* rule being bypassed;
* reason;
* risk;
* mitigation;
* responsible person;
* expiration or follow-up.

Technical debt created by an exception must be visible.

---

# 25. RHC TECH PRODUCT PRINCIPLES

Every engineering decision should reinforce the RHC Tech product philosophy.

## Clarity

The system should be understandable without a manual.

## Precision

Behavior must be intentional.

## Security

Trust is a product feature.

## Reliability

Important operations must not depend on luck.

## Performance

The product should feel immediate when technically possible.

## Accessibility

Quality is not limited to ideal users and ideal devices.

## Observability

Unknown failures are unacceptable.

## Consistency

Products belong to one ecosystem.

## Product individuality

Consistency does not mean identical products.

RHC Training, H&NTrip and Inspecto may have their own visual language, metrics and personality while remaining recognizably RHC Tech.

## Restraint

Do not add components, animations, cards, metrics or abstractions merely because they can be added.

Every element must have a purpose.

---

# 26. THE RHC TECH RULE

Before approving any implementation, ask:

> Would we be comfortable shipping this unchanged to thousands of users under the RHC Tech name?

If the answer is no:

**it is not ready.**

---

# 27. STANDARD DEVELOPMENT PIPELINE

From now on, every relevant change begins by classifying risk. The workflow then follows the controls required by Section 32.

```text
01 — Define change / problem
        ↓
02 — Classify Risk Tier
        ↓
03 — Declare Applicable Controls
        ↓
        ├──────────── R0 ─────────────┐
        │                              │
        │  Create Lightweight Record   │
        │  Apply Conditional controls  │
        │  if any surface triggers     │
        │                              │
        └──────────────┬───────────────┘
                       │
        ┌────────── R1–R4 ────────────┐
        │                              │
        │  Create required Spec        │
        │  Required reviews            │
        │  Adversarial review (solo)   │
        │  Approve                     │
        │  Implementation Plan when    │
        │  required by scope/risk      │
        │                              │
        └──────────────┬───────────────┘
                       ↓
04 — Implement within approved controls
        ↓
05 — Execute required automated/manual verification
        ↓
06 — Acceptance / evidence review at required depth
        ↓
07 — Merge
        ↓
08 — Deploy only when the change has runtime release impact
        ↓
09 — Production verification only when required/applicable
        ↓
10 — Close engineering record
```

R0 must not be forced through R1–R4 controls unless a Conditional control becomes applicable or the change is reclassified.

No implementation step may be treated as a substitute for a previous step or control that is mandatory for the assigned Risk Tier.

---
