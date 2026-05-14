# Cloud Architecture Overview

## System Context

This monorepo contains a simple full-stack TODO application with a React frontend, an Express API, and an in-memory SQLite data store. The system is intended for local development and bootcamp exercises rather than production cloud deployment.

```mermaid
flowchart LR
    user[User]
    browser[Web Browser]
    frontend[React Frontend\npackages/frontend]
    api[Express API\npackages/backend]
    store[(In-Memory SQLite Store\nbetter-sqlite3 :memory:)]

    user --> browser
    browser --> frontend
    frontend -->|HTTP requests to /api/tasks| api
    api -->|CRUD task records| store
```

## Components

- **React frontend**: Provides the TODO user interface and sends task operations to the API.
- **Express API**: Exposes task endpoints for listing, creating, updating, completing, and deleting tasks.
- **In-memory SQLite store**: Holds task data only for the lifetime of the backend process.

## Creating a TODO

```mermaid
sequenceDiagram
    actor User
    participant Browser as Web Browser
    participant Frontend as React Frontend
    participant API as Express API
    participant Store as In-Memory SQLite Store

    User->>Browser: Enters TODO details and submits form
    Browser->>Frontend: Handles form submission
    Frontend->>API: POST /api/tasks
    API->>Store: Insert task record
    Store-->>API: Return created task
    API-->>Frontend: 201 Created with task JSON
    Frontend-->>Browser: Refresh task list
    Browser-->>User: Shows the new TODO
```

## Runtime Notes

- The root workspace starts the frontend and backend together with `npm run start`.
- API calls are made under `/api/tasks` from the frontend.
- Data is not durable because the SQLite database is initialized in memory when the backend starts.
