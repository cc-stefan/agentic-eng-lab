# Task priority — feature specification

**Status:** Approved  
**Requirements source:** `docs/requirements/task-priority.md`  
**Approval owner:** Unassigned

## Outcome

Users can see each task's urgency through the task API. Priority is a small, API-only addition using three explicit values. [AC-1, AC-2]

## Scope and behavior

| Spec ID | Behavior or constraint | Source criterion |
|---|---|---|
| S-1 | Creation accepts exactly `low`, `normal`, or `high`; omission gives `normal`. | AC-1 |
| S-2 | Created and listed task responses include priority; tasks without a value are represented as `normal`. | AC-2 |
| S-3 | Other supplied values, including different casing or surrounding whitespace, are rejected with HTTP 422 in the existing validation-error style; values are not normalized. | AC-3 |
| S-4 | Omitting priority remains valid, and title validation behavior is unchanged. | AC-4 |
| S-5 | Listing gains no priority sorting or filtering. Tasks remain in memory and reset on server restart. | AC-5 |

## Out of scope

- Priority sorting or filtering, persistence, authentication, UI, and deployment changes. [AC-5]

## Verification intent

Use focused API tests and review of the observable API behavior. Existing tests already cover supported values, the default, rejection of an unsupported value, title validation, and listing a created task; add or confirm explicit cases for whitespace/case variants, missing-priority fallback, and the no-sort/no-filter scope when implementation work is authorized.

| Spec ID | Planned evidence | Evidence status |
|---|---|---|
| S-1–S-5 | API tests for creation and response shape, missing-priority fallback, exact-value validation, unchanged title validation, and unchanged listing/storage behavior. | Existing partial coverage; remaining checks planned |

## Assumptions and unresolved decisions

- “Task responses” includes the created task and each item returned by `GET /tasks`, consistent with the current API. [AC-2]
- The requirements' baseline statements [E1, E2] conflict with current `app/main.py` and `tests/test_api.py`, which already define and exercise priority. `README.md` also omits priority from its documented request and response shapes. Confirm/update those references as part of requirements review; their stale content is not treated as a behavior decision here.
- Whether a user can change priority after creation remains open. The current API has no task-update endpoint; this spec does not decide whether update capability belongs in scope.
