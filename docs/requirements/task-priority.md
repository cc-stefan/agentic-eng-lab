# Task priority — requirements worksheet

**Status:** Approved  
**Approval owner:** Requester  
**Last reviewed:** 2026-11-17

## Context and user outcome

The team needs a simple way to prioritize work so users can see task urgency. This draft specifies a priority value in the existing Task API; it is not approved for implementation.

## Sources reviewed

| Source | Evidence |
|---|---|
| Trainer-prepared GitHub Issue [#3](https://github.com/cc-stefan/agentic-eng-lab/issues/3) | Requests adding task priority so users can see what is urgent, and keeping it simple. |
| Requester answers in this conversation (2026-11-16) | Sets allowed values, default, API visibility, validation behavior, and scope constraints recorded below. |
| `app/main.py` | `TaskCreate` currently accepts `title` and `completed`; `Task` adds `id`. `POST /tasks` creates and returns a task; `GET /tasks` lists tasks, optionally filtered by completion. Tasks are in memory. |
| `tests/test_api.py`; `tests/conftest.py` | Tests cover health, empty listing, creation/default completion, whitespace-only title rejection, and listing a created task. The fixture resets the in-memory store and IDs. No priority tests exist. |
| `README.md` | Documents the current task response shape, creation fields, completion filter, and in-memory reset on restart. |
| Related repository issues [#1](https://github.com/cc-stefan/agentic-eng-lab/issues/1) and [#2](https://github.com/cc-stefan/agentic-eng-lab/issues/2) | Closed issues concern API documentation and title validation; neither requests priority. |

## Confirmed facts

- **[R1]** The request is to add task priority so users can see what is urgent and to keep the change simple. Source: Issue #3.
- **[A1]** Allowed priority values are exactly lowercase `low`, `normal`, and `high`; all other values are invalid. Source: requester answer.
- **[A2]** Priority defaults to `normal` for existing and newly created tasks. Clients that omit it during creation must continue to work. Source: requester answer.
- **[A3]** Accept priority when creating a task and return it in task responses. Source: requester answer.
- **[A4]** Invalid priority values produce HTTP 422 using the API's existing validation-error response style. Do not silently trim or normalize values. Source: requester answer.
- **[A5]** Preserve current title validation behavior. Source: requester answer.
- **[A6]** Do not add priority sorting or filtering in this release. Keep the current API-only scope and in-memory storage; tasks continue to reset on server restart. No persistence, authentication, UI, or deployment changes. Source: requester answer.
- **[E1]** Current `TaskCreate` fields are `title` and `completed`; `Task` adds `id`. `POST /tasks` returns HTTP 201 and `GET /tasks` supports an optional `completed` filter. Source: `app/main.py`.
- **[E2]** Existing tests have no priority assertions. They cover task creation and list behavior relevant to response-shape compatibility. Source: `tests/test_api.py`.

## Assumptions and inferences

| Statement | Evidence | Confirmation needed? |
|---|---|---|
| “Task responses” covers both the created task returned by `POST /tasks` and each task returned by `GET /tasks`. | [A3] plus the two current task-response endpoints in [E1]. | No, unless the requester intended a narrower response scope. |
| Priority is not editable after creation in this release. | [A3] names creation and response behavior; current API has no task-update endpoint [E1]. | Yes; see open question. |

## Acceptance criteria

1. **[AC-1; A1, A2, A3]** `POST /tasks` accepts `priority` as `low`, `normal`, or `high`; when omitted, it creates the task with `priority: "normal"`.
2. **[AC-2; A2, A3]** Task responses include priority. Tasks that predate the change or otherwise lack a priority value are represented with `priority: "normal"`.
3. **[AC-3; A1, A4]** A supplied value other than the exact lowercase strings `low`, `normal`, or `high` is rejected with HTTP 422 using the existing validation-error response style. Values with leading/trailing whitespace or different casing are not normalized into valid values.
4. **[AC-4; A2, A5, E1, E2]** A client that omits priority can still create a task successfully, and existing title validation behavior remains unchanged.
5. **[AC-5; A6]** Task listing does not gain priority sorting or filtering; storage remains in memory and resets on restart.

## Non-goals and constraints

- No priority sorting or filtering in this release.
- No persistence, authentication, UI, or deployment changes.
- Keep current in-memory storage and restart behavior.
- Preserve title validation and existing behavior outside the specified priority addition.

## Open questions

- Should users be able to change a task's priority after creation? This was not specified. The current API has no task-update endpoint, so treating priority as create-only is an inference; a different answer could expand API scope.

## Review record

- **Decision:** Approved.
- **Reviewed by:** Requester (approval in conversation, 2026-11-17).
- **Notes:** Approval covers the requirements; implementation remains pending task selection.
