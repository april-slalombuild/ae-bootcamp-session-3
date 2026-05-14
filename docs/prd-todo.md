# Product Requirements Document (PRD) - Todo App MVP Requirements

## 1. Overview

Upgrade the basic Todo app from simple title/completed tracking to a more useful task management experience. The MVP should remain lean and teachable while adding due dates, priorities, and date-based filters so users can identify urgent work without introducing backend or external storage changes.

---

## 2. MVP Scope

- Add an optional `dueDate` field for each task.
- Store `dueDate` values as ISO `YYYY-MM-DD` strings.
- Treat invalid `dueDate` values as absent.
- Add a `priority` field for each task.
- Support priority values `P1`, `P2`, and `P3`.
- Default new tasks to priority `P3`.
- Keep task `title` required.
- Add task filters for `All`, `Today`, and `Overdue`.
- In the `All` filter, show both incomplete and completed tasks.
- In the `Today` and `Overdue` filters, show only incomplete tasks.
- Keep storage local for MVP.
- Avoid backend, external storage, or multi-user changes.

---

## 3. Post-MVP Scope

- Visually highlight overdue tasks so they stand out from other tasks.
- Add priority color coding:
  - `P1`: red badge.
  - `P2`: orange badge.
  - `P3`: gray badge.
- Add task sorting rules:
  - Overdue tasks first.
  - Then priority order from `P1` to `P3`.
  - Then due date ascending.
  - Tasks without a due date last.

---

## 4. Out of Scope

- Notifications.
- Recurring tasks.
- Multi-user support.
- Keyboard navigation or special accessibility features.
- Backend changes.
- External storage.