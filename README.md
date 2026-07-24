# RHC Tech Engineering

Central source of truth for RHC Tech engineering standards, governance, playbooks, templates and risk controls.

> **Specified. Reviewed. Implemented. Verified. Reliable.**

## Current baseline

**RHC Tech Spec-Driven Development Standard v1.3**

- Canonical index: [`standards/RHC_TECH_SDD_v1.3.md`](standards/RHC_TECH_SDD_v1.3.md)
- Operational playbook: [`playbook/SDD_OPERATIONAL_PLAYBOOK.md`](playbook/SDD_OPERATIONAL_PLAYBOOK.md)
- Risk matrix/checklist: [`checklists/risk-tiers.md`](checklists/risk-tiers.md)

## Repository structure

```text
standards/
  RHC_TECH_SDD_v1.3.md
  v1.3/
    part-01-sections-00-10.md
    part-02-sections-11-27.md
    part-03-sections-28-38.md
    part-04-sections-39-43.md

playbook/
  SDD_OPERATIONAL_PLAYBOOK.md

templates/
  feature-spec.md
  r1-lite-spec.md
  lightweight-change-record.md
  adversarial-review.md
  implementation-plan.md
  verification.md
  adr.md
  bug-record.md
  exception-record.md

checklists/
  risk-tiers.md

changes/
  CHG-XXX-*.md
```

## Authority model

The Standard under `standards/` is normative. Playbooks, templates, checklists, AI prompts and product-repository conventions are operational aids.

**If an operational artifact conflicts with the Standard, the Standard wins.**

Changes that add, remove or weaken mandatory engineering obligations require a new Standard version. Operational guidance may evolve without a Standard version bump only when it does not change normative obligations.

## Products

The baseline applies to current and future RHC Tech products, currently including:

- RHC Training
- H&NTrip
- Inspecto

Product repositories progressively adopt the Standard according to the Legacy Adoption & Touch Rule rather than through forced retrospective rewrites.

## Enforcement

**No Risk Tier → No Ready.**  
**No Applicable Controls → No Ready.**  
**No Ready → No Code.**  
**No required independent review → No Approved.**  
**No evidence → No Done.**  
**No passed mandatory gates → No Release.**
