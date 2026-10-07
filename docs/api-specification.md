# TaskFlow AI — Formal API Specification & Data Contracts

> **Version:** `1.0.0`  
> **Status:** `Active / Approved`  
> **Base Path:** `/api/v1`  
> **JSON Standard:** `snake_case` keys across all request & response payloads.

---

## 1. Domain Types & Enums

### 1.1 Task Status (`status`)
Represents the current lifecycle state of a task on the Kanban board:

```typescript
export type TaskStatus = "todo" | "in_progress" | "done";
```

| Value | Description |
| :--- | :--- |
| `"todo"` | Task has been created and is awaiting action. |
| `"in_progress"` | Task is actively being worked on. |
| `"done"` | Task has been completed. |

---

### 1.2 Task Priority (`priority`)
Represents the urgency / importance level of a task:

```typescript
export type TaskPriority = "low" | "medium" | "high" | "urgent";
```

| Value | Recommended UI Color / Badge |
| :--- | :--- |
| `"low"` | Slate / Gray (`#64748b`) |
| `"medium"` | Blue (`#3b82f6`) *(Default)* |
| `"high"` | Amber / Orange (`#f59e0b`) |
| `"urgent"` | Red / Rose (`#ef4444`) |

---

## 2. Core Entities

### 2.1 Subtask (`Subtask`)
A discrete checklist item belonging to a parent task.

```json
{
  "id": "e4eaaaf2-d142-11e1-b3e4-080027620cdd",
  "title": "Design responsive navigation bar",
  "is_completed": false
}
```

| Field | Type | Required | Constraints / Description |
| :--- | :--- | :--- | :--- |
| `id` | `string` | Yes | UUID v4 identifier. |
| `title` | `string` | Yes | Min 1 char, max 200 chars. |
| `is_completed` | `boolean` | Yes | Default: `false`. Tracks completion state. |

---

### 2.2 Task (`Task`)
The primary entity representing a Kanban task card.

```json
{
  "id": "9b1deb4d-3b7d-4bad-9bdd-2b0d7b3dcb6d",
  "title": "Implement User Authentication Flow",
  "description": "Create login and signup components with form validation.",
  "status": "todo",
  "priority": "high",
  "subtasks": [
    {
      "id": "e4eaaaf2-d142-11e1-b3e4-080027620cdd",
      "title": "Design login form UI",
      "is_completed": true
    },
    {
      "id": "e4eaaaf2-d142-11e1-b3e4-080027620cde",
      "title": "Wire up submit handler",
      "is_completed": false
    }
  ],
  "created_at": "2026-10-07T22:00:00Z",
  "updated_at": "2026-10-07T22:15:00Z"
}
```

| Field | Type | Required | Default | Constraints / Description |
| :--- | :--- | :--- | :--- | :--- |
| `id` | `string` | Yes | Auto-generated | UUID v4 identifier. |
| `title` | `string` | Yes | - | Min 1 char, max 120 chars. Leading/trailing whitespace trimmed. |
| `description` | `string` | No | `""` | Max 2000 chars. Supports markdown text. |
| `status` | `TaskStatus` | No | `"todo"` | Must be one of `"todo"`, `"in_progress"`, `"done"`. |
| `priority` | `TaskPriority`| No | `"medium"` | Must be one of `"low"`, `"medium"`, `"high"`, `"urgent"`. |
| `subtasks` | `Subtask[]` | No | `[]` | List of subtask items. |
| `created_at` | `string` | Yes | Auto-generated | ISO 8601 UTC timestamp string. |
| `updated_at` | `string` | Yes | Auto-generated | ISO 8601 UTC timestamp string (updated on any mutation). |

---

## 3. REST API Endpoints

### 3.1 List All Tasks
- **Route:** `GET /api/v1/tasks`
- **Query Parameters (Optional Filters):**
  - `status` (`string`, optional): Filter by `"todo"`, `"in_progress"`, or `"done"`.
  - `priority` (`string`, optional): Filter by `"low"`, `"medium"`, `"high"`, or `"urgent"`.
- **Success Response:** `200 OK`
```json
[
  {
    "id": "9b1deb4d-3b7d-4bad-9bdd-2b0d7b3dcb6d",
    "title": "Setup SQLite database connection",
    "description": "Initialize database schema and tables.",
    "status": "done",
    "priority": "high",
    "subtasks": [],
    "created_at": "2026-10-07T22:00:00Z",
    "updated_at": "2026-10-07T22:05:00Z"
  }
]
```

---

### 3.2 Create a Task
- **Route:** `POST /api/v1/tasks`
- **Request Body (`TaskCreate`):**
```json
{
  "title": "Add Drag and Drop support",
  "description": "Enable reordering of task cards across columns.",
  "priority": "high",
  "status": "todo",
  "subtasks": []
}
```
- **Success Response:** `201 Created` $\rightarrow$ Returns full created `Task` object.
- **Error Responses:**
  - `422 Unprocessable Entity`: Missing `title`, empty `title`, or invalid `priority`/`status`.

---

### 3.3 Get Single Task by ID
- **Route:** `GET /api/v1/tasks/{id}`
- **Path Parameter:** `id` (`string`, UUID)
- **Success Response:** `200 OK` $\rightarrow$ Returns `Task` object.
- **Error Responses:**
  - `404 Not Found`: `{ "detail": "Task not found" }`

---

### 3.4 Update a Task
- **Route:** `PUT /api/v1/tasks/{id}`
- **Path Parameter:** `id` (`string`, UUID)
- **Request Body (`TaskUpdate`):**
  All fields are optional in partial updates; provided fields overwrite existing values.
```json
{
  "title": "Add Drag and Drop support (Updated)",
  "description": "Supports both mouse and touch events.",
  "status": "in_progress",
  "priority": "urgent",
  "subtasks": [
    {
      "id": "e4eaaaf2-d142-11e1-b3e4-080027620cdd",
      "title": "Install drag-and-drop package",
      "is_completed": true
    }
  ]
}
```
- **Success Response:** `200 OK` $\rightarrow$ Returns updated `Task` object with refreshed `updated_at`.
- **Error Responses:**
  - `404 Not Found`: `{ "detail": "Task not found" }`
  - `422 Unprocessable Entity`: Invalid fields or empty title.

---

### 3.5 Delete a Task
- **Route:** `DELETE /api/v1/tasks/{id}`
- **Path Parameter:** `id` (`string`, UUID)
- **Success Response:** `204 No Content` (Empty body)
- **Error Responses:**
  - `404 Not Found`: `{ "detail": "Task not found" }`

---

### 3.6 AI Subtask Decomposition
- **Route:** `POST /api/v1/tasks/{id}/ai-breakdown`
- **Path Parameter:** `id` (`string`, UUID)
- **Request Body (Optional):**
```json
{
  "max_suggestions": 5
}
```
- **Success Response:** `200 OK`
```json
{
  "task_id": "9b1deb4d-3b7d-4bad-9bdd-2b0d7b3dcb6d",
  "suggestions": [
    {
      "id": "f5cbbba3-d142-11e1-b3e4-080027620caa",
      "title": "Configure drag-and-drop context provider",
      "is_completed": false
    },
    {
      "id": "f5cbbba3-d142-11e1-b3e4-080027620cab",
      "title": "Implement drop zone animations and placeholders",
      "is_completed": false
    },
    {
      "id": "f5cbbba3-d142-11e1-b3e4-080027620cac",
      "title": "Persist column status change to backend API",
      "is_completed": false
    }
  ]
}
```

> ⚠️ **CRITICAL ARCHITECTURAL INVARIANT:**  
> The `POST /api/v1/tasks/{id}/ai-breakdown` endpoint **ONLY generates suggestions**. It **NEVER mutates the database**.  
> The client displays the suggestions in the UI checklist. The user reviews, edits, and explicitly selects which subtasks to keep. Once confirmed by the user, the frontend saves the updated subtasks list using `PUT /api/v1/tasks/{id}`.

---

## 4. Shared Code Contracts

### 4.1 TypeScript Definitions (For Developer 2 in `frontend/src/types/task.ts`)

```typescript
export type TaskStatus = "todo" | "in_progress" | "done";

export type TaskPriority = "low" | "medium" | "high" | "urgent";

export interface Subtask {
  id: string;
  title: string;
  is_completed: boolean;
}

export interface Task {
  id: string;
  title: string;
  description: string;
  status: TaskStatus;
  priority: TaskPriority;
  subtasks: Subtask[];
  created_at: string;
  updated_at: string;
}

export interface TaskCreatePayload {
  title: string;
  description?: string;
  status?: TaskStatus;
  priority?: TaskPriority;
  subtasks?: Array<Omit<Subtask, "id"> | Subtask>;
}

export interface TaskUpdatePayload {
  title?: string;
  description?: string;
  status?: TaskStatus;
  priority?: TaskPriority;
  subtasks?: Subtask[];
}

export interface AIBreakdownResponse {
  task_id: string;
  suggestions: Subtask[];
}
```

---

### 4.2 Pydantic Schemas (For Developer 3 in `backend/app/schemas.py`)

```python
from datetime import datetime
from enum import Enum
from typing import List, Optional
from pydantic import BaseModel, Field
import uuid

class TaskStatus(str, Enum):
    TODO = "todo"
    IN_PROGRESS = "in_progress"
    DONE = "done"

class TaskPriority(str, Enum):
    LOW = "low"
    MEDIUM = "medium"
    HIGH = "high"
    URGENT = "urgent"

class SubtaskBase(BaseModel):
    title: str = Field(..., min_length=1, max_length=200)
    is_completed: bool = False

class SubtaskCreate(SubtaskBase):
    id: Optional[str] = None

class Subtask(SubtaskBase):
    id: str = Field(default_factory=lambda: str(uuid.uuid4()))

    class Config:
        orm_mode = True

class TaskBase(BaseModel):
    title: str = Field(..., min_length=1, max_length=120)
    description: Optional[str] = Field(default="", max_length=2000)
    status: TaskStatus = TaskStatus.TODO
    priority: TaskPriority = TaskPriority.MEDIUM

class TaskCreate(TaskBase):
    subtasks: Optional[List[SubtaskCreate]] = []

class TaskUpdate(BaseModel):
    title: Optional[str] = Field(None, min_length=1, max_length=120)
    description: Optional[str] = Field(None, max_length=2000)
    status: Optional[TaskStatus] = None
    priority: Optional[TaskPriority] = None
    subtasks: Optional[List[Subtask]] = None

class TaskResponse(TaskBase):
    id: str
    subtasks: List[Subtask] = []
    created_at: datetime
    updated_at: datetime

    class Config:
        orm_mode = True

class AIBreakdownRequest(BaseModel):
    max_suggestions: Optional[int] = Field(default=5, ge=1, le=10)

class AIBreakdownResponse(BaseModel):
    task_id: str
    suggestions: List[Subtask]
```
