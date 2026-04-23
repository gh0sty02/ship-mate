# ShipMate

A Claude Code plugin that runs your feature through a full dev pipeline — from a story file to shipped, reviewed, and tested code.

```
/ship stories/forgot-password.md
```

---

## How it works

You write a story. ShipMate handles the rest.

```
SCAN → ORCHESTRATE → ARCHITECT → [you approve the plan]
     → IMPLEMENT → REVIEW → QA → PLAYWRIGHT (UI tasks only)
```

One human checkpoint per task — after the architect writes the plan, before any code is written. Everything else runs autonomously.

---

## Install

```bash
/plugin marketplace add gh0sty02/ship-mate
/plugin install ship-mate
```

Then in your project:

```bash
/setup
```

This scans your codebase, generates an `AGENTS.md` tailored to your project, and sets up the `stories/` folder.

---

## Usage

**Write a story** — copy `stories/_template.md`, fill it in:

```md
# Story: Forgot Password

## Description
As a user, I want to reset my password via email.

## Acceptance Criteria
- Given I'm on the login page, when I click "Forgot password", then I see a reset form
- Given I submit my email, then I receive a reset link within 60s

## Tasks
- [ ] Add "Forgot password?" link to login page
- [ ] POST /api/auth/reset-password endpoint
```

**Run the pipeline:**

```bash
/ship stories/forgot-password.md   # start
/ship status                        # check progress
/ship resume                        # continue after checkpoint
```

---

## Agents

| Agent | What it does |
|---|---|
| Orchestrator | Clarifies requirements, confirms FRONTEND or BACKEND |
| Architect | Writes a step-by-step implementation plan |
| Developer | Implements the plan, writes tests |
| PR Reviewer | Reviews code — auto-fixes minor issues, flags critical ones |
| QA | Tests every acceptance criterion and edge case |
| Playwright | Runs browser tests (FRONTEND tasks only) |

---

## Rules

- Each task is either **FRONTEND** or **BACKEND** — not both
- Review and QA loops cap at **2 iterations** before escalating to you
- `AGENTS.md` is generated once on first scan and only updated when you confirm an architectural change
- Pipeline state is saved to `.claude/pipeline/` — safe to close and resume later

---

## Requirements

Two plugins are installed automatically on first `/setup`:

- [`understand-anything`](https://github.com/Lum1104/Understand-Anything) — codebase scanning
- [`context-mode`](https://github.com/mksglu/context-mode) — keeps context window clean during scans

---

## License

MIT
