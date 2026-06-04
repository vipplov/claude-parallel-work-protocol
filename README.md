# Claude Parallel Work Protocol

A two-file system for running multiple Claude Code instances on the same repo at the same time, without them stepping on each other.

---

## The problem

Claude Code is fast. So you start running two instances in parallel to go faster. One is rebuilding your blog page. The other is fixing your analytics setup. Both need to touch `layout.tsx`. They both do. Now your layout is broken in a way that is hard to debug because two separate sets of changes got mixed together.

The standard advice is to use Git branches. That works. But if two instances start at almost the same time, or if you forget to check which files the other instance is touching, you still end up with conflicts you have to resolve manually.

This protocol solves it with a simpler approach: a markdown file that each Claude Code instance reads before it starts, and writes to before it touches anything.

---

## How it works

There are two files:

**`CLAUDE.md`** — Claude Code reads this automatically at the start of every session. The protocol instructions live here. You add them once and every future instance follows them.

**`ACTIVE_TASKS.md`** — A running log of what each Claude Code instance is doing and which files it is touching. Each instance reads this before starting, checks for conflicts, and writes its own entry before making any changes.

The flow looks like this:

1. You start a Claude Code instance with your prompt
2. Claude Code reads `ACTIVE_TASKS.md`
3. It lists every file it plans to touch
4. If any of those files appear as `IN_PROGRESS` in the log, it stops and tells you exactly what the conflict is
5. If there is no conflict, it writes its entry as `IN_PROGRESS` and starts working
6. When it finishes, it updates its entry to `DONE`

You do not need to do anything except start the instances. Claude Code handles the rest.

---

## Installation

Copy both files into the root of your repo:

```
your-repo/
├── CLAUDE.md          ← add the protocol section (or create if you do not have one)
├── ACTIVE_TASKS.md    ← create this fresh
└── ... rest of your repo
```

If you already have a `CLAUDE.md`, paste the protocol section from this repo into the top of yours. Do not replace the whole file.

---

## The one thing you need to do

Stagger your instance starts by about 30 seconds.

Start instance 1. Wait until you can see it has written its entry to `ACTIVE_TASKS.md`. Then start instance 2.

This prevents the one scenario the file-based approach cannot fully solve: two instances reading the log at the exact same millisecond before either has written to it. In practice, since you are the one starting instances manually, a short gap eliminates this entirely.

---

## What it does not solve

- If two instances start at exactly the same moment, they can both read an empty log and both proceed. The 30-second gap above prevents this.
- If a Claude Code run crashes mid-task, its entry stays as `IN_PROGRESS`. You will need to manually change it to `DONE` or `FAILED` before the next instance will proceed.
- This is not a replacement for Git branches. It is a lightweight coordination layer that works on top of however you already manage your branches.

---

## Example

You want to run two things in parallel:

- Instance 1: rebuild the blog index page
- Instance 2: add an `llms.txt` file

Instance 1 writes to `ACTIVE_TASKS.md`:

```
| blog-index-redesign | main | IN_PROGRESS | src/app/blog/page.tsx, src/components/BlogCard.tsx | 2026-06-04 17:45:00 | |
```

You start instance 2. It reads the log, sees that `src/app/blog/page.tsx` is not in its list of files (it only needs `public/llms.txt` and `src/app/sitemap.ts`), finds no conflict, and starts immediately.

If you had tried to run a second blog-related task instead, it would have seen the conflict and stopped:

```
CONFLICT: src/app/blog/page.tsx is currently IN_PROGRESS under task "blog-index-redesign" 
(started 2026-06-04 17:45:00). 

Please wait for that task to finish before starting this one, or confirm you want to proceed anyway.
```

---

## Credit

Built by [@vipplovchoudhary](https://x.com/vipplovchoudhary) while building [invoiceno.com](https://invoiceno.com), a free invoice generator for freelancers.

The idea came from needing to ship faster as a solo founder running multiple Claude Code instances at once. No servers, no databases, no CI infrastructure. Just a markdown file.
