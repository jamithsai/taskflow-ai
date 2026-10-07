# Multi-Agent Collaboration Protocol (`AGENTS.md`)

> **MANDATORY READING:** Every Antigravity agent participating in this repository MUST read and adhere to these guidelines before inspecting, creating, or editing any code or documentation.

---

## 1. Multi-Agent Operating Context

- **Team Setup:** Three human developers are building TaskFlow AI concurrently.
- **Agent Independence:** Each human developer operates with an independent Antigravity account and a separate AI session.
- **Shared Source of Truth:** The central GitHub repository (`origin`) is the single source of truth for code, documentation, and state.
- **Human Authority:** Human developers review, approve, and execute all merges on GitHub. Agents propose changes via Pull Requests but do NOT unilaterally merge pull requests without human direction.

---

## 2. Core Golden Rules for All Agents

1. **Read Before Writing:** Always read `AGENTS.md`, `ARCHITECTURE.md`, and `CONTRIBUTING.md` to understand context and constraints before beginning any task.
2. **Inspect First:** Always inspect the existing filesystem, Git branch status, and recent commits (`git status`, `git branch`, `git log -n 5`) before modifying anything.
3. **Strict Scope Focus:** Work strictly on your assigned task. Never perform unsolicited broad refactors or tangential edits.
4. **Dedicated Branches Only:** All new work must take place on a dedicated branch prefixed with `feature/`, `fix/`, `chore/`, or `docs/`.
5. **Never Push Directly to `main`:** Direct pushes to the `main` branch are strictly forbidden. Always push to a remote feature branch and open a Pull Request.
6. **Preserve Teammate Code:** Do not arbitrarily rewrite, reformat, or delete code created by other developers or agents. If an existing interface must change, document why in the PR.
7. **Minimize Cross-Boundary Churn:** Keep modifications strictly contained within your ownership area. If cross-cutting changes are required (e.g., API schemas), coordinate clearly.
8. **Test Before Proposing PRs:** Run all relevant unit/integration tests and linters locally before staging and committing.
9. **Document Architectural Changes:** Any change to API contracts, data models, or system structure must be updated in `ARCHITECTURE.md`.
10. **Transparent Reporting:** Provide clear, concise reports of files created or edited, commands run, and verification outcomes.

---

## 3. Team Ownership Boundaries

To avoid merge conflicts while enabling rapid parallel development, the codebase is partitioned into three primary ownership areas:

### 👤 Developer 1 (Lead / Integration Agent)
- **Primary Domain:**
  - Repository structure, foundation, and tooling configuration.
  - Project-wide documentation (`README.md`, `AGENTS.md`, `ARCHITECTURE.md`, `CONTRIBUTING.md`).
  - End-to-end integration across frontend and backend boundaries.
  - Cross-cutting concerns (shared types/contracts, repository health, release verification).
- **Secondary Domain:** Can assist with frontend or backend integration glue code when needed.

### 👤 Developer 2 (Frontend Agent)
- **Primary Domain:**
  - `frontend/` directory.
  - React components (Kanban board, task cards, modal dialogs, status controls).
  - Client-side routing, state management, and Tailwind / CSS styling.
  - Client-side API client services consuming backend endpoints.
- **Boundary Rule:** Must not alter backend models or routes directly. If API endpoints or response payloads need adjustment, open an issue or specify contract updates for Developer 3.

### 👤 Developer 3 (Backend + AI Agent)
- **Primary Domain:**
  - `backend/` directory.
  - FastAPI server configuration, routes, and dependency injection.
  - SQLite database schemas, connection management, and CRUD services.
  - AI Subsystem interface and pluggable task decomposition service.
  - Backend unit and endpoint tests.
- **Boundary Rule:** Must not alter frontend components or styling directly. Provide clear OpenAPI schemas and response contracts for Developer 2 to consume.

> **Integration Flexibility Note:** Ownership boundaries are designed for conflict prevention, not rigid silos. When connecting frontend components to backend endpoints, both agents may inspect adjacent code to ensure API compatibility.

---

## 4. Standard Agent Task Execution Lifecycle

When given a task, each Antigravity agent must execute the following cycle:

```
1. VERIFY BASE STATE
   - Run `git fetch origin` and ensure working on latest `main`
   - Run `git status` to verify a clean working directory

2. CREATE TOPIC BRANCH
   - Create and switch to a descriptive branch: `git checkout -b <type>/<description>`

3. IMPLEMENT WITH MINIMAL BLAST RADIUS
   - Edit/create only the files necessary for the specific task
   - Adhere to `ARCHITECTURE.md` conventions

4. VERIFY & TEST
   - Run local build, lint, and test scripts
   - Confirm no unintended files or untracked artifacts exist

5. ATOMIC CONVENTIONAL COMMITS
   - Stage target files (`git add <files>`)
   - Commit with structured messages (e.g., `feat(kanban): add drag-and-drop column layout`)

6. PUSH & PR PREPARATION
   - Push to remote branch (`git push -u origin <branch-name>`)
   - Fill out `.github/pull_request_template.md` thoroughly
   - Report status and provide the PR link to the human developer
```

---

## 5. Conflict Resolution & Communication Protocol

- **Never Force Push (`--force`):** Do not rewrite Git history on remote branches.
- **If Merge Conflicts Arise:**
  1. Fetch latest main: `git fetch origin main`
  2. Rebase or merge main into the topic branch: `git merge origin/main`
  3. Resolve conflict markers carefully, preserving the intent of both changes.
  4. Re-run tests before pushing.
- **PR Description Requirement:** Every PR must specify:
  - What task was solved.
  - Which files were modified.
  - Verification steps performed.
  - Any notes for teammate agents consuming the changes.
