# Lab Storyboard — Git Version Control Lab Sessions

**Course:** Software Engineering Lab
**Repository:** [Git Workshop Toolkit](../README.md)
**Problem set:** the 38-task Git Speedrun kit in [`speedrun/tasks.md`](../speedrun/tasks.md)

**Live board:** [Git Version Control Lab – Storyboard](https://github.com/users/AjithAM316/projects/1) (GitHub Projects: Backlog / To Do / In Progress / Testing / Done). Each user story below is an issue on that board (US1–US13, plus test-case issues).

This storyboard tracks progress through every problem attempted across the lab
sessions. Each problem is written as a short user story ("As a learner, I
want to ... so that ...") and moved across the board as it's completed. Every
story is backed by a working, seeded repo (`speedrun/cafe-project/`) so the
same commands produce the same verifiable result every time — see
[`speedrun/check-progress.sh`](../speedrun/check-progress.sh) for the
automated half of the verification.

## Board summary

| Column | Count |
|---|---|
| To Do | 0 |
| In Progress | 0 |
| **Done** | **38 / 38** |

Automated verification (via `check-progress`): **18 / 38** tasks.
Manual / instructor-observed (transient state, can't be checked after the
fact — e.g. a `git diff` you looked at, a stash you popped): **20 / 38** tasks.
See [`test-cases.md`](test-cases.md) for the full breakdown per task.

---

## Done — Base Round (Sessions 1–2)

| # | User story | Verified by |
|---|---|---|
| 1 | As a learner, I want to initialize a brand-new repo so that I can start tracking a project from scratch. | Automated |
| 2 | As a learner, I want to check what git sees before and after adding a file so that I understand tracked vs. untracked state. | Manual |
| 3 | As a learner, I want to stage a new file so that I understand the staging area. | Manual |
| 4 | As a learner, I want to commit my staged change so that I have a saved snapshot in history. | Automated |
| 5 | As a learner, I want to view commit history so that I can see what's been done before me. | Manual |
| 6 | As a learner, I want to see an unstaged diff so that I know what changed before I stage it. | Manual |
| 7 | As a learner, I want to see a staged diff so that I can distinguish staged vs. committed changes. | Manual |
| 8 | As a learner, I want to amend my last commit so that I can fix a mistake without adding a new commit. | Manual |
| 9 | As a learner, I want to create a branch so that I can work without touching `main`. | Automated |
| 10 | As a learner, I want to switch between branches so that I can move my working context. | Manual |
| 11 | As a learner, I want to fast-forward merge a branch so that I understand the simplest merge case. | Automated |
| 12 | As a learner, I want to resolve a real merge conflict by hand so that I'm not afraid of conflict markers. | Automated |
| 13 | As a learner, I want to discard an unstaged change so that I can undo a mistake before committing. | Manual |
| 14 | As a learner, I want to unstage a change without losing it so that I can rethink what to commit. | Manual |
| 15 | As a learner, I want to soft-reset a commit so that I can undo history while keeping my changes staged. | Manual |
| 16 | As a learner, I want to mixed-reset a commit so that I understand git's default undo behavior. | Manual |
| 17 | As a learner, I want to hard-reset a commit so that I understand the risk of permanently discarding work. | Manual |
| 18 | As a learner, I want to revert a commit so that I can undo shared history safely, without rewriting it. | Automated |
| 19 | As a learner, I want to stash and pop unfinished work so that I can switch context temporarily. | Manual |
| 20 | As a learner, I want to clone a repository so that I get a full working copy including all history. | Automated |

## Done — Bonus Round (Sessions 3–4)

| # | User story | Verified by |
|---|---|---|
| 21 | As a learner, I want to filter commit history by author, message, or count so that I can find relevant commits quickly. | Manual |
| 22 | As a learner, I want to pickaxe-search history for a specific string so that I can find the commit that introduced or removed it. | Automated |
| 23 | As a learner, I want to bisect history so that I can binary-search for the exact commit that broke something. | Automated |
| 24 | As a learner, I want to blame a file so that I know who last touched each line and when. | Manual |
| 25 | As a learner, I want to inspect one commit in detail so that I can see its exact message, author, and diff. | Manual |
| 26 | As a learner, I want to create lightweight and annotated tags so that I understand the difference between them. | Automated |
| 27 | As a learner, I want to inspect and add remotes so that I understand how a repo connects to multiple remotes. | Automated |
| 28 | As a learner, I want to push a new branch so that I can share my work on a remote with zero conflict risk. | Automated |
| 29 | As a learner, I want to pull remote changes so that my local branch stays in sync with the team. | Automated |
| 30 | As a learner, I want to fetch without merging so that I can inspect incoming changes before deciding what to do. | Automated |
| 31 | As a learner, I want to cherry-pick a single commit so that I can bring in one specific change without merging a whole branch. | Automated |
| 32 | As a learner, I want to squash messy WIP commits via interactive rebase so that history stays clean before sharing. | Automated |
| 33 | As a learner, I want to rebase my branch onto the latest `main` so that my branch replays cleanly on top of it. | Manual |
| 34 | As a learner, I want to recover a "lost" commit via reflog so that I never lose work permanently by accident. | Automated |
| 35 | As a learner, I want to resolve a conflict that comes up mid-rebase so that I know it's handled differently from a merge conflict. | Automated |
| 36 | As a learner, I want to stop tracking a file committed by mistake so that it stays on disk but out of git. | Automated |
| 37 | As a learner, I want to diff two branches directly so that I can compare their divergent changes. | Manual |
| 38 | As a learner, I want to recognize detached HEAD and save work from it so that I don't lose an experimental commit. | Manual |

---

## Notes

- "Automated" = confirmed by `speedrun/check-progress.sh` / `.ps1` reading git
  plumbing state (refs, objects, remotes) with no manual judgment involved.
- "Manual" = the underlying git state is transient (a diff, a stash, a reset)
  and can't be verified after the fact, so it's confirmed by walking through
  the task and observing the described "success looks like" behavior directly
  (see [`speedrun/tasks.md`](../speedrun/tasks.md) for the exact criteria per task).
- Full traceability from problem → test steps → expected result → pass/fail
  is in [`test-cases.md`](test-cases.md).
