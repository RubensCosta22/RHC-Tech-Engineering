# RHC TECH — SPEC-DRIVEN DEVELOPMENT STANDARD

## Version 1.3

**Status:** Mandatory
**Scope:** All current and future RHC Tech products
**Products currently covered:** RHC Training, H&NTrip, Inspecto

### Relevant change definition

A **relevant change** is any change that materially affects one or more of the following:

* product behavior or business rules;
* user data or persistence;
* database schema, migrations or RLS;
* APIs or external integrations;
* authentication or authorization;
* security or privacy;
* significant UX/UI behavior;
* architecture or infrastructure;
* critical observability;
* material performance or reliability characteristics.

Pure copy corrections, typo fixes, documentation-only changes and maintenance that provably does not alter runtime behavior may use a lightweight change record instead of a full Feature Spec. When in doubt, a Feature Spec is required.

---

# 1. PURPOSE

RHC Tech does not develop features directly from ideas, conversations, screenshots or isolated prompts.

Every relevant change must begin with an explicit specification.

The specification defines:

* what will be built;
* why it exists;
* how it should behave;
* what must not change;
* architecture constraints;
* security requirements;
* UX/UI requirements;
* observability requirements;
* testing strategy;
* acceptance criteria;
* objective evidence required for approval.

**Code is an implementation of the specification.
The specification is not documentation written after the code.**

---

# 2. CORE PRINCIPLE

The official RHC Tech development flow is:

**Problem → Spec → Review → Approval → Implementation → Verification → Release**

Never:

**Idea → Code → Fix → Fix again → Production**

A feature is not considered complete because it works.

It is complete only when it:

* works correctly;
* is secure;
* respects architecture;
* respects the Brand System;
* works responsively;
* handles error states;
* produces useful telemetry;
* has tests appropriate to its risk;
* does not introduce regressions;
* meets every acceptance criterion from its Spec.

---

# 3. SOURCES OF TRUTH

Every RHC Tech product operates with four primary sources of truth.

### 3.1 Product Specification

Defines product behavior and business rules.

### 3.2 Architecture Specification

Defines technical architecture, boundaries, persistence, integrations, authentication, authorization and infrastructure.

### 3.3 RHC Tech Brand System

Defines visual identity, components, typography, spacing, interaction principles, accessibility, motion and product-specific visual language.

The current official baseline is:

**RHC Tech Brand System v2.0**

Its principles apply to all products.

### 3.4 Security & Quality Standard

Defines mandatory engineering quality gates.

When implementation conflicts with any of these sources, the implementation must change.

The standard must not be relaxed merely because the implementation already exists.

---

# 4. SPEC HIERARCHY

RHC Tech uses specifications at different levels.

## L0 — Product Spec

Defines the product itself.

Examples:

* target users;
* primary problems;
* business rules;
* product boundaries;
* primary journeys;
* permissions model;
* product principles.

Created rarely and maintained throughout the product lifecycle.

---

## L1 — Domain / System Spec

Defines a major system area.

Examples:

* authentication;
* workout engine;
* travel finance;
* document management;
* statistics;
* notifications;
* admin console;
* media storage.

---

## L2 — Feature Spec

Defines a feature or meaningful change.

Most development work starts here.

---

## L3 — Implementation Plan

Defines how an approved specification will be implemented.

Includes:

* files/modules affected;
* database changes;
* migrations;
* API changes;
* component changes;
* test plan;
* rollout strategy;
* risk areas;
* change risk classification;
* required verification depth.

The Implementation Plan cannot silently change requirements established by the Feature Spec.

---

# 5. MANDATORY FEATURE SPEC TEMPLATE

Every relevant feature must contain the following sections.

## 5.1 Identification

**Spec ID**

**Title**

**Product**

**Version**

**Owner**

**Required Approvers**

**Risk Classification**

**Applicable Controls**

**SDD Process Budget Target**

**Status**

Possible states:

`Draft → In Review → Approved → In Development → Verification → Released`

A Spec may enter `Approved` only after all required reviews for its risk and scope are complete. In solo development, R1–R4 work requires the mandatory adversarial AI review defined in Section 29 before approval, as established by the Risk Tier Applicability Matrix in Section 32.

---

## 5.2 Problem

Describe the problem being solved.

Do not describe the implementation.

Bad:

> Add another card to the dashboard.

Good:

> Users cannot quickly identify their weekly training consistency without opening the statistics center.

---

## 5.3 Objective

Define the desired result.

The objective must be measurable or objectively verifiable whenever possible.

---

## 5.4 Non-Goals

Explicitly document what is outside the scope.

This exists to prevent uncontrolled scope expansion.

---

## 5.5 User Stories / Use Cases

Describe the relevant user journeys.

Example:

> As an athlete, I want to see my weekly training consistency so that I can understand whether I am following my plan.

---

## 5.6 Functional Requirements

Each requirement receives an identifier.

Example:

**FR-01**
The system must display the number of workouts completed during the current week.

**FR-02**
The weekly period must respect the user's configured timezone.

**FR-03**
A user must never see workout information belonging to another profile without explicit permission.

Requirements must be testable.

---

## 5.7 Business Rules

Business logic must be explicit.

Never leave important business behavior for the developer to infer.

---

## 5.8 Permissions & Authorization

Every specification involving user data must define:

* who can read;
* who can create;
* who can update;
* who can delete;
* who can administer;
* what happens when access is denied.

For Supabase applications, authorization cannot rely only on frontend checks.

RLS and server/database enforcement must be evaluated explicitly.

---

# 6. UX/UI SPECIFICATION

UI must never be implemented based only on vague instructions such as:

> Make it premium.

Every user-facing feature must define:

### User journey

Where the user comes from and what happens next.

### Information hierarchy

What is:

* primary;
* secondary;
* contextual;
* optional.

### States

At minimum, when applicable:

* loading;
* empty;
* success;
* validation error;
* system error;
* unauthorized;
* offline/degraded;
* disabled.

### Responsive behavior

Desktop and mobile behavior must be intentionally defined.

Desktop must not simply be an enlarged mobile layout.

### Accessibility

At minimum:

* keyboard navigation;
* semantic structure;
* focus visibility;
* readable typography;
* sufficient contrast;
* meaningful labels.

### Brand compliance

Every interface must comply with the RHC Tech Brand System.

Product-specific accent colors and visual language must also be respected.

**Data must inform, not decorate.**

Graphs, statistics and dashboards must communicate:

* value;
* context;
* comparison;
* period;
* trend.

Decoration without information is not acceptable.

---

# 7. ARCHITECTURE SPECIFICATION

Before implementation, changes with architectural impact must document:

### Components affected

### Data flow

### Dependencies

### Storage

### Database impact

### External integrations

### Authentication

### Authorization

### Failure modes

### Scalability implications

### Backward compatibility

### Migration strategy

Important architectural decisions must be recorded as ADRs.

---

# 8. SECURITY BY SPEC

Security review happens before implementation, not only during audit.

Every Spec must answer the relevant questions:

### Authentication

Who is the user?

### Authorization

What exactly can the user access?

### Data ownership

Who owns each entity?

### Data exposure

Can data belonging to another account become visible?

### Input validation

What data can enter the system?

### File security

For uploads:

* permitted MIME types;
* size limits;
* storage path;
* access policy;
* signed URLs when necessary;
* malicious filename handling;
* metadata handling.

### Secrets

Secrets must never be committed or exposed to clients when not explicitly intended to be public.

### Sensitive data

Passwords, access tokens, refresh tokens, authorization headers, secrets and personal information must never be written to logs without an explicit and safe reason.

### Abuse cases

The Spec must consider misuse and privilege escalation.

---

# 9. OBSERVABILITY BY DEFAULT

Production code must not fail silently.

Critical operations must produce useful telemetry.

Production backend and server-side services must use structured logging for critical operations.

**JSON is the default structured log format unless an ADR explicitly approves another format.**

Recommended format:

**JSON structured logs**

Expected context when applicable:

* timestamp;
* level;
* service;
* environment;
* requestId;
* userId or safe internal identifier;
* action;
* resource;
* duration;
* outcome;
* error code.

Log levels:

`info`

Normal relevant system activity.

`warn`

Unexpected but recoverable condition.

`error`

Operation failure requiring investigation.

`fatal`

Failure affecting process or essential service operation.

Sensitive information must be sanitized before logging.

A professional logging system such as **Pino or Winston** must be adopted when the application architecture includes server-side or backend execution requiring production telemetry.

At minimum, logging redaction must protect:

* passwords;
* access and refresh tokens;
* authorization headers;
* cookies and session identifiers;
* secrets and API keys;
* payment-card data;
* sensitive personal data;
* any credential or value that could enable account or system access.

Redaction must happen before data is emitted to logs.

---

# 10. FAILURE HANDLING

The following pattern is prohibited:

```text
try
  operation
catch
  ignore
```

Errors must have an intentional outcome.

Possible valid outcomes include:

* user receives a meaningful message;
* operation retries safely;
* fallback is activated;
* telemetry is generated;
* transaction is rolled back;
* error is propagated to an appropriate boundary.

Every critical failure path must be observable.

---
