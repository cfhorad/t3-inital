# Architecture Documentation 🩻

This document outlines the software architecture, database relations, role-based access control, and image upload workflow for the **RadCases** application.

---

## 1. Architectural Overview

RadCases follows a monorepo-style Next.js App Router architecture integrated with tRPC for end-to-end type-safe queries and mutations.

```mermaid
graph TD
    Client[Client Component - Next.js/HeroUI] -- tRPC Query/Mutation --> API[tRPC Router / Server-Side]
    Client -- Direct Upload --> UT[UploadThing Storage]
    API -- Read/Write --> DB[(PostgreSQL Database)]
    API -- Authenticate Upload --> UT
```

---

## 2. Role-Based Access Control (RBAC)

Authentication is powered by **Better Auth**, which manages session tokens and maps them to a `users` table. 

### Roles & Permissions Matrix
| Action | Student | Teacher | Admin |
| :--- | :---: | :---: | :---: |
| Browse / Search Cases | ✅ | ✅ | ✅ |
| Rate Cases (1-3 stars) | ✅ | ✅ | ✅ |
| Comment & Like | ✅ | ✅ | ✅ |
| Upload Radiologic Case | ❌ | ✅ | ✅ |
| Manage User Roles | ❌ | ❌ | ✅ |

### Middleware & tRPC Integration
*   **`protectedProcedure`**: Extends standard procedures to ensure the user is logged in.
*   **`teacherProcedure`**: Enforces that `ctx.session.user.role` is either `'teacher'` or `'admin'`.
*   **`adminProcedure`**: Enforces that `ctx.session.user.role` is `'admin'`.

Users register as `'student'` by default. The Admin elevates user roles via the Admin Dashboard.

---

## 3. Database Schema Design (Drizzle ORM)

The relational schema is configured in `src/server/db/schema.ts` as follows:

```mermaid
erDiagram
    users ||--o{ cases : "authors"
    users ||--o{ case_comments : "writes"
    users ||--o{ case_likes : "likes"
    users ||--o{ case_ratings : "rates"
    
    acr_anatomy ||--o{ cases : "categorizes"
    acr_pathology ||--o{ cases : "categorizes"
    
    cases ||--o{ case_images : "contains"
    cases ||--o{ case_comments : "has"
    cases ||--o{ case_likes : "has"
    cases ||--o{ case_ratings : "has"
    
    users {
        string id PK
        string email
        string role "admin | teacher | student"
    }
    acr_anatomy {
        string id PK "e.g., '06'"
        string name "e.g., 'Lung, Mediastinum and Pleura'"
    }
    acr_pathology {
        string id PK "UUID or string"
        string name "e.g., 'Inflammation/Infection'"
    }
    cases {
        string id PK
        string title
        string diagnosis
        text notes
        string author_id FK
        string acr_anatomy_id FK
        string acr_pathology_id FK
        string difficulty "beginner | intermediate | advanced"
        timestamp created_at
    }
    case_images {
        string id PK
        string case_id FK
        string url "UploadThing URL"
        string type "test | answer"
        integer order
    }
    case_comments {
        string id PK
        string case_id FK
        string user_id FK
        text content
        timestamp created_at
    }
    case_likes {
        string case_id PK, FK
        string user_id PK, FK
    }
    case_ratings {
        string case_id PK, FK
        string user_id PK, FK
        integer stars "1-3"
    }
```

---

## 4. UploadThing Image Flow

To support large radiologic files efficiently, files are uploaded directly from the browser to UploadThing.

```mermaid
sequenceDiagram
    autonumber
    actor Teacher as Teacher (Browser)
    participant Server as Next.js UploadThing Route
    participant UT as UploadThing Bucket
    participant DB as Postgres (Drizzle)

    Teacher->>Server: Request Upload Auth
    Note over Server: Verify user has 'teacher' or 'admin' role
    Server->>Teacher: Return secure token
    Teacher->>UT: Direct File Upload
    UT-->>Teacher: Upload Complete (URL returned)
    Teacher->>DB: Save Case details & Images array
```

---

## 5. Querying & Performance Strategy

### Case Filtering & Search
*   **Partial Text Search:** Cases are searched using case-insensitive SQL `ilike` operations against the `title` and `notes` columns.
*   **Structured Filtering:** Filters filter records exactly matching `acr_anatomy_id`, `acr_pathology_id`, and `difficulty`.

### Aggregate Analytics
Drizzle Relational Queries (or SQL joins) aggregate:
1.  **Average Star Rating:** Calculated dynamically using `avg(case_ratings.stars)` and rounded to 1 decimal place.
2.  **Likes Count:** Count of entries in `case_likes` matching the `case_id`.
3.  **User State:** Checks whether the current requester has already liked/rated the case, indicating the state of the UI like button.
