# Task priority — implementation task list

**Status:** Approved  
**Specification:** `docs/specs/task-priority.md`  
**Requirements:** `docs/requirements/task-priority.md`

This planning approval does not authorize implementation; wait for the requester to select a task.

| Task ID | Verifiable result | Acceptance criteria | Planned verification | Depends on | Status |
|---|---|---|---|---|---|
| T-1 | Task creation accepts the three exact priority values, defaults omitted priority to `normal`, rejects other values with the existing 422 validation style, and preserves optional-priority and title-validation compatibility. | AC-1, AC-3, AC-4 | Planned focused API tests for each accepted value, omission, invalid casing/whitespace and another unsupported value, plus title-validation regression behavior. | — | Not started |
| T-2 | Created and listed task responses include priority, representing tasks without a value as `normal`. | AC-2 | Planned API/compatibility checks of both response paths, including a task lacking priority. | — | Not started |
| T-3 | Priority does not change task-list sorting/filtering or the in-memory, restart-reset storage behavior. | AC-5 | Planned review of the observable listing behavior and storage lifecycle; focused API checks for listing with and without the existing completion filter. | — | Not started |

## Traceability check

| Acceptance criterion | Task IDs | Verification evidence | Covered or deferred? |
|---|---|---|---|
| AC-1 | T-1 | Accepted values and omitted-priority default API tests. | Covered |
| AC-2 | T-2 | Created/listed response checks, including missing-priority fallback. | Covered |
| AC-3 | T-1 | Invalid exact-value, casing, and whitespace API tests; assert HTTP 422 style. | Covered |
| AC-4 | T-1 | Omitted-priority creation and title-validation regression tests. | Covered |
| AC-5 | T-3 | Listing behavior checks and storage-lifecycle review. | Covered |

No criteria are uncovered or deferred; all verification evidence is planned, not claimed as existing. No task is unrelated to the acceptance criteria. No dependencies are required between these tasks.

## Open decision

Whether priority can be changed after task creation remains unresolved in the specification. No task for update behavior is included unless that decision changes scope.

## Scope changes

Keep this aligned with the scope-change record in `docs/specs/task-priority.md`.

- None recorded.
