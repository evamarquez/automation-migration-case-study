# Project brief

## Business problem

The team had accumulated 46 Make automations around an Airtable-based operating system. Those scenarios encoded business rules, duplicate protection, notifications, folder creation, onboarding, offboarding, and timing assumptions.

A direct module-by-module translation could preserve the visible blueprint while losing behaviour hidden in filters, field mappings, schedules, and error branches.

## Goal

Document and rebuild the automation estate around Supabase while preserving important behaviour, making risks explicit, and validating cutover readiness.

## Scope

- Inventory of 46 automations and dependencies
- Blueprint and live-scenario comparison
- Field and data-model mapping
- Sixteen selected scenario rebuilds
- Idempotency, permissions, error paths, and schedule parity
- Backups, defect log, validation, and handoff

## Constraints

- Client names, record IDs, credentials, field identifiers, and live endpoints remain private.
- The live original is the behavioural source of truth during comparison.
- Rebuilt and productionized is not the same as activated in live production.
- Behavioural parity comes before optional improvements.

## Definition of done

- Every selected flow has an owner, status, and dependency record.
- Rebuilt scenarios match validated production behaviour.
- Duplicate, permission, schedule, and rollback risks are documented.
- The team can make an informed activation decision.
