# Migration design

## Delivery model

The migration followed a repeatable loop:

1. Inventory the whole automation estate.
2. Read the live original, not only an exported blueprint.
3. Map fields, dependencies, schedules, and error branches.
4. Back up the pre-change blueprint.
5. Rebuild the selected scenario against Supabase.
6. Compare behaviour and validate representative cases.
7. Document defects, decisions, status, and handoff.

## Source-of-truth rule

The exported blueprint was useful evidence, but it was not sufficient by itself. The live scenario exposed operational details that could disappear in a copy: hidden filters, stale routes, message destinations, schedule differences, and first-time-only guards.

## Key decisions

### Preserve behaviour before improving it

The first rebuild aimed for behavioural parity. Improvements were documented as decisions instead of being mixed into the migration invisibly.

### Make writes idempotent

Events that could arrive twice used a stable dedupe key and a database constraint, so retries did not create duplicate tasks, messages, or folders.

### Move privileged work behind the connection

Service-role-only database calls were moved through the configured Supabase connection rather than leaving an anonymous key in a module header.

### Back up before replacing

Blueprint backups created a concrete before-and-after comparison and a recovery path if an activated rebuild behaved differently.
