# Active Tasks — Example

> This is an example file showing what ACTIVE_TASKS.md looks like during a real parallel run.
> Copy the blank version from ACTIVE_TASKS.md into your repo, not this one.

---

## Example: two tasks running cleanly in parallel

These two tasks ran at the same time with no conflict because they touched completely different files.

| Task | Branch | Status | Files Touched | Started | Completed |
|------|--------|--------|---------------|---------|-----------|
| setup-parallel-protocol | main | DONE | CLAUDE.md, ACTIVE_TASKS.md | 2026-06-04 17:00:00 | 2026-06-04 17:01:00 |
| blog-index-redesign | main | DONE | src/app/blog/page.tsx, src/components/BlogCard.tsx | 2026-06-04 17:45:00 | 2026-06-04 18:20:00 |
| add-llms-txt | main | DONE | public/llms.txt, public/llms-full.txt, src/app/sitemap.ts | 2026-06-04 17:46:00 | 2026-06-04 17:52:00 |

---

## Example: conflict detected and reported to the user

This is what Claude Code outputs when it detects a conflict before starting:

```
I read ACTIVE_TASKS.md before starting.

The files I need to touch for this task are:
- src/app/blog/page.tsx
- src/components/BlogCard.tsx

CONFLICT: src/app/blog/page.tsx is currently IN_PROGRESS under task "blog-index-redesign"
started at 2026-06-04 17:45:00.

I have not made any changes. Please wait for that task to complete and its status 
to update to DONE, then run this prompt again. Or let me know if you want me to 
proceed anyway.
```

---

## Example: a stuck entry that needs manual cleanup

If a Claude Code run crashes mid-task, its entry stays as IN_PROGRESS. 
Change it to FAILED manually so other instances can proceed.

| Task | Branch | Status | Files Touched | Started | Completed |
|------|--------|--------|---------------|---------|-----------|
| fix-analytics | main | FAILED | src/components/Analytics.tsx | 2026-06-04 19:00:00 | |

After marking it FAILED, the next instance will no longer see it as a blocker.
