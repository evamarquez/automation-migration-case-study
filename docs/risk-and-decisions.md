# Risk and decision register

| Risk | Potential effect | Control |
|---|---|---|
| Hidden workflow rules | A visible translation loses operational behaviour. | Compare the rebuild with the live original. |
| Duplicate events | Repeated tasks, messages, or folders. | Stable dedupe keys and database constraints. |
| Privilege mismatch | Service-role calls fail after cutover. | Route privileged work through the configured connection. |
| Schedule drift | Webhooks or jobs run at the wrong time. | Validate schedule parity against the original. |
| Unsafe replacement | A new blueprint cannot be recovered. | Back up before changing the active scenario. |
| Premature activation | Unvalidated behaviour reaches production. | Separate rebuild, productionization, and activation status. |
| Incomplete handoff | The estate depends on its original builder. | Keep dependencies, owners, defects, and decisions documented. |

## Decision rules

1. Treat behaviour, not module shape, as the migration target.
2. Preserve existing behaviour before making improvements.
3. Record a reason for every retired, blocked, replaced, or out-of-scope flow.
4. Keep activation as an explicit team decision after validation.
