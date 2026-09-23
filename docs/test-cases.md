# Test Cases — Git Speedrun Lab Kit

**Course:** Software Engineering Lab
**Module under test:** the Git Speedrun kit (`speedrun/setup-team.sh`/`.ps1`
generating the seeded `cafe-project` repo, exercised via the 38 tasks in
`speedrun/tasks.md`, verified by `speedrun/check-progress.sh`/`.ps1`)
**Test environment:** a team folder produced by `setup-team.sh <TEAMNAME>`,
containing `warmup/` (empty), `cafe-project/` (9 seeded commits + 6 feature
branches), and `cafe-project-origin.git` (bare "remote").

Each row is one test case: preconditions, the exact steps/commands run
(input), the expected result, how it's verified, and the resulting status.
"Automated" cases are asserted mechanically by `check-progress`; "Manual"
cases are transient git state (a diff, a stash, a reset) that can't be
re-inspected after the fact, so they're verified by walking through the step
and confirming the documented behavior at the time.

| TC ID | Task | Objective | Test Steps (Input) | Expected Result | Verification | Status |
|---|---|---|---|---|---|---|
| TC01 | 1 | `git init` creates a new repo | `cd warmup && git init` | `warmup/.git` exists; `git status` no longer errors with "not a git repository" | Automated | Pass |
| TC02 | 2 | `git status` reflects tracked/untracked state | Run `git status` on empty repo; create `notes.txt`; run `git status` again | 1st run: "nothing to commit". 2nd run: `notes.txt` listed as untracked | Manual | Pass |
| TC03 | 3 | `git add` stages a new file | Write text into `notes.txt`; `git add notes.txt` | `git status` shows `notes.txt` under "Changes to be committed" | Manual | Pass |
| TC04 | 4 | `git commit` saves a snapshot | `git commit -m "Add notes"` | `git log` shows exactly one commit with that message | Automated | Pass |
| TC05 | 5 | `git log` shows full history | `git log`; `git log --oneline` (in `cafe-project`) | 9 commits visible, oldest = "Initial commit" | Manual | Pass |
| TC06 | 6 | `git diff` shows unstaged changes | Edit `menu.txt`, don't stage; `git diff` | Diff output shows `-`/`+` lines for the edit | Manual | Pass |
| TC07 | 7 | `git diff --staged` shows staged changes | Edit `menu.txt`; `git add menu.txt`; `git diff --staged` | Diff appears under `--staged`; plain `git diff` is empty | Manual | Pass |
| TC08 | 8 | `git commit --amend` edits the last commit | Commit a small change; `git commit --amend -m "<new message>"` | Same commit count in `git log`, new message, new hash | Manual | Pass |
| TC09 | 9 | `git branch <name>` creates a branch pointer | `git branch scratch` | `git branch` lists `scratch`; current branch is still `main` | Automated | Pass |
| TC10 | 10 | `git switch`/`checkout` moves between branches | `git switch scratch`; `git branch`; `git switch main` | `*` marker moves to `scratch` then back to `main` | Manual | Pass |
| TC11 | 11 | Fast-forward `git merge` | One commit on `scratch`; `git switch main`; `git merge scratch` | Git reports "Fast-forward"; no new merge commit created; `scratch` is now an ancestor of `main` | Automated | Pass |
| TC12 | 12 | Resolve a real merge conflict | `git switch feature/happy-hour`; `git merge feature/weekend-special`; edit `menu.txt` to remove `<<<<<<<`/`=======`/`>>>>>>>` markers on the Latte line; `git add menu.txt`; `git commit` | Working tree clean, no conflict markers remain, `git log --graph` shows a merge commit with 2 parents | Automated | Pass |
| TC13 | 13 | `git restore <file>` discards an unstaged change | Edit `about.txt`, don't stage; `git restore about.txt` | `git diff` empty; file matches last commit | Manual | Pass |
| TC14 | 14 | `git restore --staged <file>` unstages without discarding | Edit `about.txt`; `git add about.txt`; `git restore --staged about.txt` | File shown as modified-but-unstaged; edit is preserved | Manual | Pass |
| TC15 | 15 | `git reset --soft` undoes a commit, keeps staged | Commit small change; `git reset --soft HEAD~1` | `git log` has one fewer commit; change still staged | Manual | Pass |
| TC16 | 16 | `git reset --mixed` undoes a commit, unstages it | Commit again; `git reset --mixed HEAD~1` | Commit gone from log; change present but unstaged | Manual | Pass |
| TC17 | 17 | `git reset --hard` undoes a commit and discards the change | Commit again; `git reset --hard HEAD~1` | Commit gone from log; working tree fully clean, edit is gone | Manual | Pass |
| TC18 | 18 | `git revert <commit>` safely undoes a commit | `git revert <hash>` on any non-initial commit on `main` | New commit on top with message starting "Revert"; original commit still visible in history | Automated | Pass |
| TC19 | 19 | `git stash` / `git stash pop` shelve work temporarily | Edit a file, don't commit; `git stash`; `git status`; `git stash pop` | After stash: edit disappears from `git status`. After pop: edit reappears | Manual | Pass |
| TC20 | 20 | `git clone` copies a repo with full history | `git clone cafe-project-origin.git cafe-project-clone` | `cafe-project-clone/` exists with a working copy; its `git log` matches origin's history | Automated | Pass |
| TC21 | 21 | `git log` filters narrow history | `git log --author="Sam Rivera"`; `git log --grep="BUG"`; `git log -n 3` | Author filter → exactly 2 commits; grep filter → the 2 "BUG:" commits | Manual | Pass |
| TC22 | 22 | `git log -S` (pickaxe) finds a text-introducing/removing commit | `git log -S "4.05" --oneline`; record the BUG commit's hash as `task22=<hash>` in `ANSWERS.md` | Two commits reported ("BUG: typo in latte price" and "Fix latte price"); recorded hash matches the known-correct answer key | Automated | Pass |
| TC23 | 23 | `git bisect` binary-searches for the breaking commit | `git bisect start`; `git bisect bad HEAD`; `git bisect good <hash>`; `git bisect run bash ../bisect-check.sh`; record result as `task23=<hash>` in `ANSWERS.md` | Git reports the "BUG: broken contact email" commit as first bad; recorded hash matches the answer key | Automated | Pass |
| TC24 | 24 | `git blame` attributes each line | `git blame about.txt` | Line with the broken contact email attributed to the correct commit and author (Sam Rivera) | Manual | Pass |
| TC25 | 25 | `git show <commit>` inspects one commit fully | `git show` on the "Fix latte price" commit hash | Output shows the exact line change ($4.05 → $4.50) with message and author | Manual | Pass |
| TC26 | 26 | `git tag` creates lightweight and annotated tags | `git tag v0.1-lightweight`; `git tag -a v0.1-annotated -m "First stable menu"` | `git tag` lists both; `git show v0.1-annotated` displays the message; lightweight tag does not | Automated | Pass |
| TC27 | 27 | `git remote -v` inspects and extends remotes | `git remote -v`; `git remote add backup ../cafe-project-origin.git` | `git remote -v` now lists both `origin` and `backup` pointing at the same bare repo | Automated | Pass |
| TC28 | 28 | `git push` sends a new branch to a remote | `git push origin feature/loyalty-card` | Push succeeds with no errors/rejection; branch exists on remote and matches local hash | Automated | Pass |
| TC29 | 29 | `git pull` fetches and merges in one step | `git pull origin main` (on `main`) | New commit fixing the contact-page typo (authored by Jordan Lee) appears in local `main`; local `main` hash matches remote `main` | Automated | Pass |
| TC30 | 30 | `git fetch` downloads without merging | `git fetch origin`; compare `git log main` vs `git log origin/main` | `refs/remotes/origin/main` tracking ref matches the real remote `main` hash, independent of whether local `main` has merged it yet | Automated | Pass |
| TC31 | 31 | `git cherry-pick` copies a single commit | On `main`: `git cherry-pick <feature/social-links commit hash>` | `main` gains a new commit with the same patch content (same patch-id) as the source commit; `feature/social-links` itself unchanged | Automated | Pass |
| TC32 | 32 | Interactive rebase squashes commits | `git switch feature/wip-styles`; `git rebase -i HEAD~3`; mark 2nd & 3rd as `squash` | `feature/wip-styles` is exactly 1 commit ahead of its merge-base with `main`; `styles-notes.txt` contains all 3 original edits | Automated | Pass |
| TC33 | 33 | `git rebase main` replays a branch onto the latest main | Still on `feature/wip-styles`: `git rebase main` | Completes cleanly with no conflicts (near-no-op since `main` hasn't moved since the branch was created) | Manual | Pass |
| TC34 | 34 | `git reflog` recovers an orphaned commit | `git reflog`; find the "Experimental: dark mode toggle" entry; `git switch -c recovered-dark-mode <hash>` | New branch exists whose tip commit message is exactly "Experimental: dark mode toggle" | Automated | Pass |
| TC35 | 35 | Resolve a conflict raised mid-rebase | `git switch feature/pricing-update`; `git rebase main`; resolve the Muffin-line conflict in `menu.txt`; `git add menu.txt`; `git rebase --continue` | Rebase finishes with no more conflicts; working tree clean; no conflict markers remain; `main` is an ancestor of the rebased branch | Automated | Pass |
| TC36 | 36 | `.gitignore` + `git rm --cached` untracks a mistaken file | On `main`: create `.gitignore` containing `debug.log`; `git rm --cached debug.log`; commit both changes | `debug.log` still present on disk; no longer listed in `git ls-tree`; `.gitignore` on `main` contains `debug.log` | Automated | Pass |
| TC37 | 37 | `git diff branchA..branchB` compares branches directly | `git diff feature/happy-hour..feature/weekend-special` | Diff shows both branches' differing edits to the same Latte line side by side | Manual | Pass |
| TC38 | 38 | Detached HEAD is recognized and saved before loss | Check out an old commit by hash directly; make a small commit; `git switch -c my-experiment` before switching away | Git warns of detached HEAD on checkout; new branch `my-experiment` exists pointing at the experimental commit | Manual | Pass |

## Summary

| Metric | Value |
|---|---|
| Total test cases | 38 |
| Automated (via `check-progress.sh`/`.ps1`) | 18 |
| Manual / instructor-observed (transient state) | 20 |
| Passed | 38 |
| Failed | 0 |

Automated cases can be re-run at any time, non-destructively, against a live
team folder with:

```bash
./speedrun/check-progress.sh path/to/TEAMNAME
```

which prints `[PASS]`/`[FAIL]`/`[SKIP]` per task plus a total score.
