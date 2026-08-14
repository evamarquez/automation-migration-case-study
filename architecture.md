# Architecture: automation migration

This document expands the public case study without reproducing private blueprints or schema details.

## Migration model

```text
                 +----------------------+
                 |  Existing automation |
                 |  live behaviour      |
                 +----------+-----------+
                            |
                            v
                 +----------+-----------+
                 |  Inventory and diff  |
                 |  modules, fields,    |
                 |  filters, schedules  |
                 +----------+-----------+
                            |
                            v
                 +----------+-----------+
                 |  Behaviour map       |
                 |  keep / swap / drop  |
                 +----------+-----------+
                            |
                            v
                 +----------+-----------+
                 |  Supabase-backed     |
                 |  rebuilt scenario    |
                 +----------+-----------+
                            |
                            v
                 +----------+-----------+
                 |  Validation and      |
                 |  cutover checklist   |
                 +----------------------+
```

## What was compared

Each scenario was reviewed across:

- Trigger type and schedule.
- Every module and route.
- Filters and first-time-only guards.
- Field mappings and dedupe behaviour.
- Database reads, writes, and privileged calls.
- Slack and Drive side effects.
- Error handlers and dead-letter paths.
- Before and after blueprint structure.

## Behavioural parity

The migration protocol treated the live Airtable scenario as evidence of behaviour, not as a clean specification. The rebuilt flow was compared against it before any decision to simplify or improve it.

This prevented silent changes such as replacing a private message with a team-channel message, setting an archive flag before a folder move, or changing a webhook schedule to a delayed poll.

## Safety controls

- Pre-change blueprint backups.
- Stable dedupe keys and database uniqueness where retries were possible.
- Supabase connections for privileged operations.
- Per-module error handling for notifications and writes.
- Explicit inactive status during validation.
- A decision log for retired and blocked automations.

## Migration states

```text
inventoried -> mapped -> rebuilt -> compared -> validated -> cutover
                                                   |
                                                   +-> blocked / needs owner decision
```

The sixteen selected scenarios reached the validated state in the testing environment. They were not represented as live after cutover because cutover had not happened.

