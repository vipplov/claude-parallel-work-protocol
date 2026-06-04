# CLAUDE.md

> This is a template. Replace the sections below with your own project details.
> Keep the ## Parallel Work Protocol section exactly as written — do not change it.

---

## Parallel Work Protocol

Before starting any task, do the following in this exact order:

1. Read `ACTIVE_TASKS.md` in the repo root
2. List every file you plan to create or modify for this task
3. Check if any of those files appear in `ACTIVE_TASKS.md` with status `IN_PROGRESS`
4. If there is a conflict: tell the user exactly which task is blocking and which file is the conflict, then stop. Do not proceed until the user gives explicit instruction.
5. If there is no conflict: write your entry to `ACTIVE_TASKS.md` with status `IN_PROGRESS`, your task name, a timestamp to seconds, and every file you plan to touch. Then start the task.
6. When the task is complete: update your entry in `ACTIVE_TASKS.md` to status `DONE` with a completion timestamp.
7. Never delete entries from `ACTIVE_TASKS.md`. Only update the Status and Completed columns.

`ACTIVE_TASKS.md` is managed automatically. Edit it without asking for confirmation.

---

## About This Project

[Replace this section with a description of your project — what it is, what tech stack it uses, and any important constraints Claude Code should know about.]

---

## Commands

[Replace this section with the commands to run your project — build, dev server, tests, lint.]

---

## Standing Instructions

[Replace this section with any standing instructions for Claude Code — how to work, what to avoid, coding conventions, etc.]
