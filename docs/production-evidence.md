# Production evidence

## Measured results

| Measure | Result |
|---|---|
| Automations inventoried | 46 |
| Selected scenarios rebuilt and productionized | 16 of 16 |
| Defects documented | 10 |
| Example retrieval improvement | Roughly 170 database operations to 1 |
| Validation example | 85 records returned in 455 ms |
| Backlink-to-slot improvement | Roughly 8-10 operations to 2 |

## Defects found

The comparison and validation work surfaced issues that a clean blueprint translation would have missed:

- A dedupe key would collide on the second event.
- A first-time-only welcome guard had disappeared.
- A state flag was no longer being written.
- A trailing space made an alert lookup return no rows.
- A private message was pointed at a team channel.
- An archive flag could be set before the folder was archived.
- Several calls used an anonymous key against service-role-only functions.
- Webhook schedules did not match the originals.

## Status boundary

The rebuilt scenarios were left inactive in the testing environment pending cutover. The case therefore claims documented, rebuilt, validated, and productionized work—not an unqualified claim that every flow was active in live production.
