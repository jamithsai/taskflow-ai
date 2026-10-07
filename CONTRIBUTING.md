# Contributing to TaskFlow AI 🤝

Welcome to the TaskFlow AI development team! This guide explains our workflow, Git commands, and collaboration practices in simple, step-by-step terms suitable for developers of all experience levels.

---

## 📖 Git Concepts in Plain English

If you are new to Git and GitHub, here is what the core terms mean:

| Term | What It Means | Everyday Analogy |
| :--- | :--- | :--- |
| **Repository (Repo)** | The folder containing all your project files and the entire history of changes. | The project storage room. |
| **Branch** | An isolated copy/line of development where you can make changes without affecting the main codebase. | A parallel draft of a document where you can experiment safely. |
| **Commit** | A saved snapshot or checkpoint of your code changes with an explanatory message. | Saving a game checkpoint or saving a document version. |
| **Push** | Uploading your saved local commits to the shared repository on GitHub. | Uploading your local draft to cloud storage. |
| **Pull** | Downloading the latest changes made by teammates from GitHub to your local machine. | Downloading the latest edits shared by your team. |
| **Pull Request (PR)** | A proposal sent to your team on GitHub asking them to review and merge your branch into `main`. | Submitting an article draft to an editor for review before publishing. |
| **Merge** | Combining the code and commits from one branch into another (e.g. into `main`). | Gluing the approved draft into the master copy. |
| **Merge Conflict** | Happens when two developers edit the same lines of the same file in different ways and Git needs human help to decide which changes to keep. | Two people editing the exact same sentence at the same time. |

---

## 🔄 The 9-Step Development Workflow

Every task follows this simple 9-step cycle:

```
[1. Pull latest main] ➔ [2. Create branch] ➔ [3. Make changes]
                                                        ↓
[6. Push branch]      ➔ [5. Commit changes] ➔ [4. Test changes]
       ↓
[7. Open PR]          ➔ [8. Review PR]      ➔ [9. Merge to main]
```

### Step 1: Get the latest `main` branch
Before starting any new work, make sure your local computer has all the recent updates:
```bash
git checkout main
git pull origin main
```
*Why? This prevents building on outdated code and avoids future merge conflicts.*

---

### Step 2: Create a branch for your task
Create and switch to a new branch for the specific task you are working on:
```bash
git checkout -b feature/kanban-board-layout
```
*Why? Isolating your work on a branch ensures `main` remains working and stable.*

---

### Step 3: Make your changes
Write your code, add components, or update documentation inside your area of ownership.

---

### Step 4: Test the changes
Ensure your code runs and tests pass locally before saving a commit.
- For frontend: Run the build/lint commands in `frontend/`.
- For backend: Run the test suite with `pytest` in `backend/`.

---

### Step 5: Commit the changes
Stage the files you modified and save a snapshot with a clear message:
```bash
# Check what files you changed
git status

# Stage the files you want to include in this checkpoint
git add frontend/src/components/KanbanBoard.tsx

# Create the commit checkpoint
git commit -m "feat(kanban): add 3-column board layout"
```

---

### Step 6: Push the branch to GitHub
Upload your local branch and commits to GitHub:
```bash
git push -u origin feature/kanban-board-layout
```
*Why? This makes your branch visible on GitHub so your team can see it.*

---

### Step 7: Open a Pull Request (PR)
1. Go to your repository on [GitHub](https://github.com/jamithsai/taskflow-ai).
2. You will see a prompt: **"Compare & pull request"**. Click it.
3. Fill out the PR template checklist (describe what you built, how you tested it, and link the issue).
4. Click **"Create pull request"**.

---

### Step 8: Review the Pull Request
Teammates and their AI assistants inspect the code changes, check for regressions, and comment on any needed adjustments.

---

### Step 9: Merge only after approval
Once approved and all checks pass, the human developer clicks **"Merge pull request"** on GitHub.

---

## 🌿 Branch Naming Conventions

Use lowercase names with forward slashes and hyphens. Pick the prefix that best matches your task:

| Prefix | Use For | Example |
| :--- | :--- | :--- |
| `feature/` | New features or user-facing functionality | `feature/ai-subtask-generator` |
| `fix/` | Bug fixes or defect repairs | `fix/task-card-drag-drop` |
| `chore/` | Maintenance, tooling, config, dependencies | `chore/setup-fastapi-backend` |
| `docs/` | Documentation changes or guides | `docs/update-architecture-diagram` |
| `refactor/`| Code restructuring without changing behavior | `refactor/task-service-sqlite` |

---

## ✍️ Conventional Commit Guidelines

Write commit messages that are clear, concise, and structured. We use the following standard format:

```
<type>(<optional scope>): <short description in present tense>
```

### Commit Types:
- `feat:` A new feature (`feat(api): add POST /tasks endpoint`)
- `fix:` A bug fix (`fix(ui): correct badge color for urgent priority`)
- `docs:` Documentation updates (`docs: update setup steps in README`)
- `chore:` Tooling, setup, or dependency updates (`chore: add project gitignore`)
- `test:` Adding or updating tests (`test(backend): add tests for task CRUD`)
- `refactor:` Code cleanup without changing external behavior (`refactor: modularize sqlite connection helper`)

---

## ⚠️ How to Handle a Merge Conflict

If GitHub says "This branch has conflicts that must be resolved", don't panic! Here is how to fix it:

1. On your machine, make sure you have the latest `main`:
   ```bash
   git checkout main
   git pull origin main
   ```
2. Switch back to your feature branch:
   ```bash
   git checkout feature/your-branch-name
   ```
3. Merge `main` into your feature branch:
   ```bash
   git merge main
   ```
4. Git will mark conflict sections in affected files with `<<<<<<<`, `=======`, and `>>>>>>>`.
5. Open the files in your editor, choose the correct combined code, and delete the Git conflict markers.
6. Stage and commit the resolved files:
   ```bash
   git add .
   git commit -m "chore: resolve merge conflicts with main"
   git push origin feature/your-branch-name
   ```
7. GitHub will automatically update your PR to show the conflict is resolved!
