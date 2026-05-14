# Epics and Stories - Todo App

## MVP

- Epic: Task Due Dates
  - Story: Add optional due date to tasks
  - Story: Store due dates in ISO format
  - Story: Ignore invalid due dates

- Epic: Task Priorities
  - Story: Add priority to tasks
  - Story: Support P1 P2 P3 priorities
  - Story: Default new tasks to P3

- Epic: Task Validation
  - Story: Require task title

- Epic: Task Filters
  - Story: Add All filter
  - Story: Add Today filter
  - Story: Add Overdue filter
  - Story: Show completed tasks in All filter
  - Story: Hide completed tasks in Today filter
  - Story: Hide completed tasks in Overdue filter

- Epic: Local Storage Scope
  - Story: Keep task storage local
  - Story: Avoid backend storage changes

## MVP Technical Requirements

### Current Implementation Baseline

- Frontend task creation and editing currently happen in `packages/frontend/src/TaskForm.js`.
- Frontend task display, completion toggling, deletion, and API loading currently happen in `packages/frontend/src/TaskList.js`.
- App-level save orchestration currently happens in `packages/frontend/src/App.js`.
- Backend task storage and CRUD endpoints currently live in `packages/backend/src/app.js` using an in-memory SQLite database.
- Existing task records currently include `id`, `title`, `description`, `due_date`, `completed`, and `created_at`.
- Existing frontend and backend code use the API field name `due_date`; the PRD uses the product field name `dueDate`.

### Task Due Dates

- Preserve due dates as optional task data in create and edit flows.
- Store accepted due dates as ISO calendar date strings in `YYYY-MM-DD` format.
- Normalize any UI-facing due date value to the existing API field `due_date` unless the implementation also updates the API contract consistently.
- Treat empty, missing, malformed, or non-calendar date values as no due date and persist them as `null` or an empty form value as appropriate for the layer.
- Ensure due date parsing is calendar-date based and does not shift dates because of time zone conversion.
- Continue displaying due dates in `TaskList` only when a valid due date exists.
- Add or update frontend tests for creating, editing, displaying, and omitting due dates.
- Add or update backend API tests for accepting valid due dates and treating invalid due dates as absent.

### Task Priorities

- Add priority as task data across create, edit, display, and storage flows.
- Support only the priority values `P1`, `P2`, and `P3`.
- Default new tasks to `P3` when the user does not choose a priority or when existing records do not have one.
- Reject or normalize unsupported priority values so task records never expose priorities outside `P1`, `P2`, or `P3`.
- Extend the current task payloads returned by `GET /api/tasks`, `GET /api/tasks/:id`, `POST /api/tasks`, and `PUT /api/tasks/:id` to include priority if the app continues to use the existing API-backed architecture.
- Add a priority input to `TaskForm` and preserve the selected value when editing an existing task.
- Render the priority value in `TaskList` in a minimal MVP-friendly way; color-coded badge styling remains Post-MVP scope.
- Add frontend tests for default priority, selected priority submission, edit-state priority loading, and priority rendering.
- Add backend tests for priority persistence, defaulting to `P3`, and validation of allowed values.

### Task Validation

- Keep the existing required title validation in `TaskForm` before saving.
- Keep the existing backend title validation on `POST /api/tasks` and `PUT /api/tasks/:id` if the current API remains part of the MVP implementation.
- Trim title values for validation so whitespace-only titles are rejected.
- Do not require description, due date, or priority selection beyond the default `P3` behavior.
- Add or keep frontend and backend tests that verify whitespace-only titles are rejected.

### Task Filters

- Add filter state to the frontend task list experience with exactly three MVP filter options: `All`, `Today`, and `Overdue`.
- The `All` filter must show incomplete and completed tasks.
- The `Today` filter must show only incomplete tasks whose valid due date equals the user's current local calendar date.
- The `Overdue` filter must show only incomplete tasks whose valid due date is before the user's current local calendar date.
- Tasks with absent or invalid due dates must not appear in `Today` or `Overdue`.
- Completed tasks must not appear in `Today` or `Overdue`, even when their due date matches those filters.
- Implement filter behavior in the frontend unless a future story explicitly expands backend filtering; the current backend only supports completion and search query filters.
- Keep the existing API fetch behavior compatible with `TaskList`, then apply MVP date filters to the loaded task array.
- Add frontend tests that cover each filter option, completed-task exclusion for date filters, and invalid or absent due dates.

### Local Storage Scope

- Do not add external persistence, hosted databases, authentication, or multi-user data ownership.
- Keep task data local to the current app runtime. In this codebase, that currently means the existing in-memory SQLite backend remains acceptable for local development and tests.
- Limit backend changes, if needed, to the existing local task model and API contract required by due date and priority fields.
- Do not introduce browser localStorage, external storage services, or new backend infrastructure unless the product scope changes.
- Preserve the existing workspace scripts and test entry points for frontend and backend validation.

## Post-MVP

- Epic: Overdue Task Highlighting
  - Story: Highlight overdue tasks visually

- Epic: Priority Badge Styling
  - Story: Add red badge for P1 tasks
  - Story: Add orange badge for P2 tasks
  - Story: Add gray badge for P3 tasks

- Epic: Task Sorting
  - Story: Sort overdue tasks first
  - Story: Sort tasks by priority
  - Story: Sort tasks by due date ascending
  - Story: Sort undated tasks last