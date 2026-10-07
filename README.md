# TaskFlow AI 🚀

TaskFlow AI is a modern, lightweight team task-management application featuring an interactive Kanban board and AI-assisted task breakdown.

---

## 📌 Project Overview

TaskFlow AI is designed to help small development teams organize, prioritize, and manage their daily workflow effortlessly. Beyond traditional task tracking, TaskFlow AI incorporates an isolated AI service that helps developers decompose large, ambiguous user stories or tasks into clear, actionable subtasks with a single click.

---

## ✨ Planned Features

- **Interactive Kanban Board:** Visual columns for task states (`To Do`, `In Progress`, `Done`).
- **Task Management:** Create, view, edit, and delete tasks with rich details.
- **Priority & Status Tracking:** Assign priority levels (`Low`, `Medium`, `High`, `Urgent`) and track workflow status.
- **Detailed Descriptions:** Markdown-supported descriptions for clear acceptance criteria.
- **AI Task Breakdown:** Intelligent backend subtask generator that analyzes task descriptions and suggests structured action items.

---

## 🛠️ Technology Stack

| Layer | Technology | Purpose |
| :--- | :--- | :--- |
| **Frontend** | React + TypeScript + Vite | Responsive, fast, type-safe single-page application |
| **Backend** | Python + FastAPI | High-performance, async REST API with automatic OpenAPI docs |
| **Database** | SQLite | Lightweight, file-based relational database |
| **AI Subsystem** | Pluggable Python Service | Modular backend interface for subtask decomposition |

---

## 📂 Project Structure

```text
taskflow-ai/
├── .github/
│   ├── ISSUE_TEMPLATE/
│   │   ├── bug.md                  # Bug report template
│   │   ├── feature.md              # Feature request template
│   │   └── task.md                 # General task template
│   └── pull_request_template.md    # Standard PR checklist & template
├── frontend/                       # React + TypeScript + Vite web app (Coming Soon)
├── backend/                        # FastAPI REST API & AI Service (Coming Soon)
├── .gitignore                      # Git ignore rules for Node, Python, and OS files
├── AGENTS.md                       # Multi-agent collaboration rules & boundaries
├── ARCHITECTURE.md                 # High-level architecture & system design
├── CONTRIBUTING.md                 # Git workflow & contribution guide for beginners
└── README.md                       # Project documentation & overview
```

---

## 👥 Team & Ownership

This project is built collaboratively by three developers, each paired with their own Antigravity AI assistant:

- **Developer 1 (Lead / Integration):** Repository foundation, cross-cutting documentation, API contract alignment, and end-to-end integration.
- **Developer 2 (Frontend):** React user interface, Kanban board components, state management, and client-side API integration.
- **Developer 3 (Backend + AI):** FastAPI endpoints, SQLite data models, business logic, and the isolated AI task-breakdown service.

---

## 🤖 Multi-Agent Collaboration Model

Because multiple Antigravity AI agents work on this shared repository simultaneously across independent workstations, all agents adhere to strict rules outlined in [`AGENTS.md`](./AGENTS.md):

1. **GitHub is the Source of Truth:** Always pull latest changes from `main` before branching.
2. **Dedicated Feature Branches:** All changes must happen on branch topics (`feature/*`, `fix/*`, `chore/*`).
3. **Never Push Directly to `main`:** All merges to `main` occur via GitHub Pull Requests.
4. **Respect Ownership Boundaries:** Minimize changes to code outside your designated area.
5. **Human Approval:** Human developers hold final review and merge authority.

---

## 🚀 Getting Started (Workflow Overview)

For a complete step-by-step guide on how our team uses Git, creates branches, opens pull requests, and writes conventional commit messages, please read [`CONTRIBUTING.md`](./CONTRIBUTING.md).

For architectural diagrams and data flows, please review [`ARCHITECTURE.md`](./ARCHITECTURE.md).

---

## 📄 License

This project is developed for educational and collaborative engineering practice.
