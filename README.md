# Automation migration: Airtable to Supabase

[View the visual project portfolio](https://www.by-evamarquez.com/portfolio). This repository contains sanitized delivery documentation, not private application source code.

How I documented and rebuilt a production automation estate without treating a blueprint export as the source of truth.

This is a sanitized case study. The source code, client names, field identifiers, record IDs, credentials, and live endpoints are not published.

## At a glance

| | |
|---|---|
| Role | AI Engineer and migration owner |
| Inventory | 46 automations documented with dependencies and migration notes |
| Rebuild | 16 of 16 production scenarios rebuilt and productionized |
| Findings | 10 defects documented during comparison and validation |
| Example reduction | 170 database operations to 1 |
| Validation example | 85 records returned in 455 ms |

## The problem

The team had accumulated a large set of Make scenarios around an Airtable-based operating system. The scenarios were not just integrations. They encoded business rules, duplicate protection, notifications, folder creation, onboarding, offboarding, and the timing assumptions of the wider system.

A direct translation would preserve the visible modules while losing the behaviour hidden in filters, field mappings, schedules, and error branches.

The migration needed to answer two questions for every flow:

1. What does the current automation actually do?
2. What must remain true after the data layer changes?

## What I built

- A 46-automation inventory with dependencies, field mappings, owners, and migration status.
- A repeatable protocol for reading the whole blueprint, comparing it with the live original, mapping fields to Supabase, rebuilding, testing, and documenting the result.
- Sixteen rebuilt scenarios using Supabase-backed reads and writes.
- Backups of pre-change blueprints so a rebuild could be compared or rolled back.
- Error handlers, dead-letter paths, duplicate protection, and schedule parity where appropriate.
- A decision log for flows that were retired, blocked, already replaced, or explicitly out of scope.

## Architecture

```text
event, webhook, or scheduled trigger
                |
                v
          Make scenario
                |
      +---------+---------+
      |                   |
      v                   v
  Supabase lookup     validation / dedupe
      |                   |
      +---------+---------+
                v
       Supabase write or RPC
                |
        +-------+-------+
        |               |
        v               v
   Slack / Drive     error path
```

The expanded model is in [architecture.md](architecture.md).

## Design decisions

### Diff against the live original

The most valuable validation step was comparing the rebuilt scenario with the scenario that was actually running. Reading the new blueprint by itself missed behaviour that was only visible in the old one.

That comparison found bugs such as a trailing space that made a lookup return no rows, an alert routed to a test channel, a first-time-only guard that had disappeared, and a dedupe key that was constant instead of unique.

### Preserve behaviour before improving it

The first objective was behavioural parity. Schedules, filters, routes, and user-facing messages were preserved unless there was evidence that they were wrong or unsafe. Improvements were documented as decisions instead of being mixed into the migration invisibly.

### Make writes idempotent

Where an event could arrive twice, the rebuild used a stable dedupe key and a database constraint. The goal was that a retry would be safe rather than create a duplicate task, message, or folder.

### Move privileged work behind the connection

Some original flows attempted to call service-role-only database functions with an anonymous key. Those calls were guaranteed to fail after cutover. The rebuild moved the privileged request through the configured Supabase connection rather than leaving a key in a module header.

### Back up before replacing

Blueprints were saved before changes. That gave the team a concrete before-and-after comparison and a recovery path if a rebuild behaved differently after activation.

## Results

- 46 automations were documented with their dependencies and migration notes.
- 16 of 16 selected production scenarios were rebuilt and productionized.
- Ten defects were documented and corrected or explicitly left open with a reason.
- One retrieval flow went from roughly 170 operations to one database request, returning 85 records in 455 ms in validation.
- Another backlink-to-slot flow went from roughly 8-10 operations to two.
- The final validation work consumed two Make operations against a monthly allowance of 20,000.

The sixteen rebuilt scenarios were left inactive in the testing environment pending cutover. That distinction matters: rebuilt and productionized is not the same as activated in live production.

## The defects were part of the result

The migration surfaced more than syntax differences:

- A dedupe key based on an application identifier would have collided on the second event.
- A first-time-only filter had been lost, so a welcome message could repeat.
- A state flag was no longer being written.
- A lookup with one trailing space made an alert route effectively dead.
- A private message had been pointed at a team channel.
- An archive flag could be set before the underlying folder was actually archived.
- Several calls used an anonymous key against service-role-only functions.
- Webhook scenarios had schedules that did not match the originals.

Finding these issues before cutover was more valuable than claiming that the migration was a clean one-to-one translation.

## What I owned

I owned the inventory, source-of-truth decisions, blueprint comparison, field mapping, rebuild protocol, validation, backups, defect log, and handoff documentation. Activation decisions and production cutover remained with the team.

## Privacy boundary

See [privacy.md](privacy.md) for the exact boundary around what is intentionally omitted.

## Project documentation

Read the migration evidence as a case study:

- [Project brief](docs/project-brief.md)
- [Migration design](docs/migration-design.md)
- [Risk and decision register](docs/risk-and-decisions.md)
- [Production evidence](docs/production-evidence.md)
- [Close-out report](docs/close-out-report.md)

The [documentation index](docs/README.md) explains how the artefacts fit together.
