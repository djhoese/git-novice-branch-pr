# SLIM.md — Two-session slim tutorial spec

This file specifies a **slimmed-down, two-session** version of this Git lesson,
each session targeting **~1 hour**. It is the source of truth for what the slim
sessions contain and how to maintain them. (For general repository/build
documentation, see `CLAUDE.md`.)

## Goal & scope

- Condense the workshop into **two ~1-hour follow-along sessions** for novice
  programmers, with students encouraged to review the full episodes for depth.
- **Session 1 — Git Basics (local):** full episodes **01–08**.
- **Session 2 — Collaborating with GitLab:** SSH setup + full episodes **09–10**.
- **Out of scope:** episode **11 ("Open") and everything after it** (licensing,
  citation, hosting, supplemental RStudio). Not included in either session.
- The natural split between sessions is between `08-conflict` and `09-gitlab`.

## Deliverables

| File | Purpose |
|------|---------|
| `_episodes/98-session-1-git-basics.md` | Session 1, a rendered Carpentries episode |
| `_episodes/99-session-2-gitlab-collaboration.md` | Session 2, a rendered episode |
| `SLIM.md` | This spec |
| `CLAUDE.md` | General repo docs (no mention of the slim sessions) |

## Key decisions

1. **Delivered as new episodes, not a fork/branch or separate site.** The slim
   sessions are added as ordinary files in `_episodes/`. This was chosen to
   **minimize maintenance and merge-conflict risk**: they touch **no existing
   files**, so re-syncing from upstream won't conflict with them.

2. **Rendering is automatic.** `_config.yml` defines no `episode_order`, so the
   site sorts episodes **alphanumerically by filename**
   (`_includes/manual_episode_order.html`). New `_episodes/*.md` files therefore
   render into the schedule, navbar, and all-in-one page with **zero edits
   elsewhere**. See `CLAUDE.md` → "Episode ordering".

3. **Filenames `98-` / `99-`.** Numbered high so the two sessions sort **after**
   all existing episodes (01–15) and leave room for future upstream episodes
   without collisions.

4. **SSH setup moved to Session 2.** SSH key generation is the bulk (~25 min) of
   the original `02-setup.md`, but it's only needed for GitLab. Session 1 keeps
   only quick `git config`; SSH moves to the start of Session 2, right before the
   first `git push`. This is what makes both sessions fit in an hour.

5. **Follow-along pacing.** Students type commands with the instructor
   throughout, on the running `planets` / `mars.txt` example (Session 1) and the
   `countries` project (Session 2).

6. **Challenges kept but uncounted.** Most challenge exercises are retained, but
   marked "(Optional)" and the episodes set `exercises: 0` in front matter so the
   schedule clock reflects **teaching time only**. The instructor decides live,
   per group and time available, whether to run each challenge.

### Accepted cosmetic caveat

The schedule table (`_includes/syllabus.html`) uses **one cumulative clock across
all episodes**. Because the slim episodes sit after episodes 01–15, they appear
at the bottom of the schedule with an inflated cumulative start time, alongside
the full lessons. Fixing this would require adding an `episode_order` list to
`_config.yml` (editing an existing file → reintroduces merge-conflict/maintenance
surface), so we deliberately leave it. The `teaching:` value on each slim episode
(≈55 min) is still correct as the per-session duration.

## Content map (source → slim, keep/cut)

Command and output text is copied from the source episodes so it stays accurate;
prose and asides are trimmed. Figures are reused from `fig/`.

### Session 1 (`98-session-1-git-basics.md`, teaching: 55)

| Source | Kept | Cut / deferred |
|--------|------|----------------|
| `01-basics` (+ `index.md` framing) | Why version control (3 bullets), `play-changes.svg` | PhD-comic tangent, paper-writing challenge |
| `02-setup` | `git config` name/email/color/editor, `init.defaultBranch main`, `git config --list` | **SSH → Session 2**, line-endings, editor table, proxy/password-manager callouts |
| `03-create` | `git init`, `.git`, `git status` | Nested-repo challenge |
| `04-changes` | modify → `add` → `commit` → `status` → `diff` → `log`; staging area; figures | multi-line log-paging callouts, author/committer, directories callout; challenges optional |
| `05-history` | `HEAD`/`HEAD~n`, `git diff <commit>`, `git checkout HEAD <file>` restore | `git show`, `git revert` detail; most challenges optional |
| `06-ignore` | `.gitignore`, commit it | negation/`**`/order challenges (optional) |
| `07-branches` | `git branch`, `checkout`/`-b`, commit on branch, `merge` (fast-forward), `branch -d`/`-D` | second (bash) branch walk-through condensed |
| `08-conflict` | create conflict, read markers, resolve, `add` + `commit`; conflict-reduction tips | second no-conflict merge demo; create-a-conflict challenge optional |

### Session 2 (`99-session-2-gitlab-collaboration.md`, teaching: 55)

| Source | Kept | Cut / deferred |
|--------|------|----------------|
| `02-setup` (SSH portion) | check `~/.ssh`, `ssh-keygen -t ed25519`, `cat ...pub`, add key in GitLab UI, `ssh -T` | Ed25519-legacy, copy/paste callout, proxy/password-manager callouts (link out) |
| `09-gitlab` | create project (uncheck README), `remote add origin`, `remote -v`, `push`, `pull`, "2nd laptop" clone/push/pull sync | unrelated-histories deep dive (link out); proxy/password-manager callouts; `-u` callout |
| `10-merge-requests` | fork, clone origin, `remote add upstream`, `pull upstream main`, `checkout -b`, add file, `commit`, `push origin <branch>`, open MR, follow-up commit updates MR; fork/clone/origin/remote/upstream terms | GitHub-era screenshots (mostly already commented out); add-another-country challenge optional |

## Maintenance notes

- The slim episodes are **additive** — they modify no upstream files, so pulling
  updates from upstream should not conflict with them.
- If upstream substantially changes the commands/outputs in episodes 01–10,
  update the two slim episodes to match (use the content map above for
  provenance).
- Preview locally with `make serve` (or `bundle exec jekyll serve`) and confirm
  the two sessions appear in the schedule and render correctly.

## Appendix: detailed cut-list

What was removed from each original episode to produce the slim versions. The
guiding rule: keep the core command flow of the running example, drop
asides/callouts/duplicate demos, and defer most challenges (only three are kept,
all marked optional).

### Session 1 (from episodes 01–08)

- **01 · Automated Version Control** — *Kept:* the "why version control" bullets
  and the `play-changes.svg` idea. *Removed:* the PhD Comics image/link, the
  "Long History of Version Control" callout (RCS/CVS/SVN), the "Paper Writing"
  challenge, and the `versions.svg` / `merge.svg` diagrams.
- **02 · Setting Up Git** — *Kept:* `git config` for name/email/color,
  `core.editor` (nano example only), `init.defaultBranch main`,
  `git config --list`. *Removed:* the Line Endings / `autocrlf` callout, the full
  editor table (12 editors → 1 example), the "Exiting Vim" callout, the long
  `master`→`main` history callout, **the entire SSH section (moved to Session
  2)**, and the proxy / password-manager callouts.
- **03 · Creating a Repository** — *Kept:* `git init`, `.git`, `git status`.
  *Removed:* the `git checkout -b main` step (unneeded once `init.defaultBranch`
  is set) and the whole "Places to Create Git Repositories" nested-repo challenge
  + solution.
- **04 · Tracking Changes** — *Kept:* the full modify → `add` → `commit` →
  `status` → `diff` → `diff --staged` → `log` cycle and both staging figures.
  *Removed:* the "Where Are My Changes?", "Word-based diffing", "Paging the Log",
  "Limit Log Size" (`-N`/`--oneline`/`--graph`), and "Directories/.gitkeep"
  callouts; and the "Committing Changes", "Committing Multiple Files", and
  "Author and Committer" challenges. Kept only "Choosing a Commit Message"
  (optional).
- **05 · Exploring History** — *Kept:* `HEAD`/`HEAD~n`, `git diff <commit>`, and
  `git checkout HEAD <file>` to restore, plus `git-checkout.svg`. *Removed:*
  `git show`, the 40-char-ID walkthrough (condensed to "first few characters"),
  the "Don't Lose Your HEAD" and "Simplifying the Common Case" callouts, the
  `git_staging.svg` cartoon, and the `git revert`, workflow-prediction,
  `git diff` prediction, staged-changes, and `git log --patch` challenges. Kept
  only "Recovering Older Versions" (optional).
- **06 · Ignoring Things** — *Kept:* creating dummy files, writing `.gitignore`
  (`*.dat`, `results/`), and committing it. *Removed:* the `git add -f` force
  demo, `git status --ignored`, and all five challenges (nested files, `!`
  negation, directory globs, rule order, log files).
- **07 · Branches** — *Kept:* `git branch`, `checkout`/`-b`, commit on a branch,
  fast-forward `merge`, and `branch -d`/`-D`. *Removed:* the entire **second
  (`bashdev`) branch walkthrough** and the repeated `ls`/`git log` verification
  steps between every switch.
- **08 · Conflicts** — *Kept:* creating the conflict across `main`/`marsTemp`,
  reading the `<<<`/`===`/`>>>` markers, resolving, and `add`+`commit`, plus the
  conflict-reduction tips. *Removed:* the **second "polar caps" change +
  no-conflict merge demo**, the "Still seeing a conflict?" callout, the repeated
  `git log --oneline` displays, and one of the two overlapping project-management
  tip lists. Kept the "create a conflict yourself" challenge (optional, solution
  trimmed).

### Session 2 (from SSH setup + episodes 09–10)

- **02 · SSH portion** — *Kept:* the `~/.ssh` check, `ssh-keygen -t ed25519`,
  `cat …pub`, the GitLab UI steps, and `ssh -T`. *Removed:* the "Keeping your
  keys secure" and "Ed25519 legacy" callouts, the verbose randomart/fingerprint
  output, the intermediate `ls`, the copy/paste callout, the deliberate *failed*
  `ssh -T` demo, and the proxy / password-manager callouts.
- **09 · Remotes in GitLab** — *Kept:* creating the project (README unchecked),
  `remote add origin`, `remote -v`, `push`, `pull`, and the "second laptop"
  clone/push/pull sync, plus the push-vs-commit challenge. *Removed:* the
  commented-out GitHub screenshots, the "server does `mkdir/git init`" aside, the
  local/remote diagrams, the "HTTPS vs SSH" and "`-u` flag" callouts, the proxy /
  password-manager callouts, the verbose push output, and the long "unrelated
  histories" README challenge.
- **10 · Merge Requests** — *Kept:* fork → clone `origin` → `remote add upstream`
  → `pull upstream main` → `checkout -b` → edit file → `commit` →
  `push origin <branch>` → open MR → follow-up commit updates MR, plus the
  fork/clone/origin/remote/upstream terminology. *Removed:* all commented-out
  GitHub screenshots, the "Project owners", "Why does it say USERNAME", and "If
  you tried copying the command" callouts (condensed to inline notes), and the
  verbose git output blocks. Kept "add another country + MR" (optional).

### Cross-cutting removals

Applied throughout: the proxy and password-manager callouts, most `{: .callout}`
asides, duplicate verification steps (`ls` + `git log` after every action), long
sample output blocks (trimmed to the informative lines), and GitHub-era
screenshots. All of this is recoverable — each session ends with "Want more
detail?" links back to the full episodes.
