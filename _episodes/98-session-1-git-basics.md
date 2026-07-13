---
title: "Slim Session 1: Git Basics (Local)"
teaching: 55
exercises: 0
questions:
- "What is version control and why should I use it?"
- "How do I set up Git and create a repository?"
- "How do I record, review, and undo changes?"
- "How can I work in parallel using branches, and resolve conflicts?"
objectives:
- "Configure Git and create a local repository."
- "Go through the modify-add-commit cycle and inspect history."
- "Restore old versions of files and ignore files you don't want to track."
- "Create, merge, and delete branches, and resolve a merge conflict."
keypoints:
- "Version control is like an unlimited 'undo' and lets many people work in parallel."
- "`git init` creates a repository; `git add` stages changes; `git commit` records them."
- "`git status`, `git diff`, and `git log` show you what has changed and when."
- "`git restore` restores old versions; `.gitignore` tells Git what to skip."
- "Branches let you work without disturbing `main`; `git merge` brings work back together."
- "Conflicts are marked in the file for you to resolve, then `git add` + `git commit`."
---

> ## About this slim session
>
> This is a condensed, ~1-hour **follow-along** version covering the local-Git
> essentials. It draws from the full episodes 1–8. For more detail, examples,
> and extra exercises, work through the full lessons linked at the end.
>
> Challenge boxes below are **optional** — your instructor may skip them
> depending on time and experience level; they are not counted in the timing.
{: .prereq}

## 1. Why version control?

Version control keeps track of every change to a set of files. It's better than
mailing files back and forth or keeping `report-final-v2-really-final.doc`:

- Nothing committed is ever really lost — you can always go back in time.
- You get a record of who changed what, when, and why.
- Several people can work in parallel, and the system flags conflicts instead of
  silently overwriting work.

Think of it as saving the *changes* to a base document, so you can replay or
combine different sets of changes.

![Changes Are Saved Sequentially](../fig/play-changes.svg)

## 2. Setting up Git (configuration only)

The first time you use Git on a computer, set your identity and preferences. We
use `git verb` commands. Use **your own** name and email:

~~~
$ git config --global user.name "Vlad Dracula"
$ git config --global user.email "vlad@tran.sylvan.ia"
~~~
{: .language-bash}

Set your preferred text editor (this is the editor Git opens for commit
messages). For example, for nano:

~~~
$ git config --global core.editor "nano -w"
~~~
{: .language-bash}

Make `main` the default branch name for new repositories, to match GitLab:

~~~
$ git config --global init.defaultBranch main
~~~
{: .language-bash}

These settings are stored per-user (that's what `--global` means) and only need
to be done once per machine. Check them any time with:

~~~
$ git config --list
~~~
{: .language-bash}

> ## SSH is in Session 2
>
> Connecting to GitLab also needs an **SSH key**. We don't need it for local
> work, so we set it up at the start of Session 2, right before pushing to
> GitLab.
{: .callout}

## 3. Creating a repository

Make a directory for our work and turn it into a Git repository:

~~~
$ mkdir planets
$ cd planets
$ git init
~~~
{: .language-bash}

`git init` creates a hidden `.git` directory that stores the project's history.
If you ever delete it, you lose the history. Confirm it exists and check status:

~~~
$ ls -a
$ git status
~~~
{: .language-bash}

~~~
On branch main
No commits yet
nothing to commit (create/copy files and use "git add" to track)
~~~
{: .output}

## 4. Tracking changes: add, commit, diff, log

Create a file `mars.txt` (use whatever editor you like — here `nano`):

~~~
$ nano mars.txt
$ cat mars.txt
~~~
{: .language-bash}

~~~
Cold and dry, but everything is my favorite color
~~~
{: .output}

`git status` now shows `mars.txt` as an **untracked** file. Tell Git to track it
by adding it to the **staging area**, then **commit** it to record a permanent
snapshot:

~~~
$ git add mars.txt
$ git commit -m "Start notes on Mars as a base"
~~~
{: .language-bash}

~~~
[main (root-commit) f22b25e] Start notes on Mars as a base
 1 file changed, 1 insertion(+)
 create mode 100644 mars.txt
~~~
{: .output}

The `-m` flag records a short, descriptive message. `git status` now reports a
clean working directory, and `git log` shows the commit:

~~~
$ git log
~~~
{: .language-bash}

~~~
commit f22b25e3233b4645dabd0d81e651fe074bd8e73b
Author: Vlad Dracula <vlad@tran.sylvan.ia>
Date:   Thu Aug 22 09:51:46 2013 -0400

    Start notes on Mars as a base
~~~
{: .output}

Now edit the file again and review the change **before** saving it with
`git diff`:

~~~
$ nano mars.txt
$ cat mars.txt
~~~
{: .language-bash}

~~~
Cold and dry, but everything is my favorite color
The two moons may be a problem for Wolfman
~~~
{: .output}

~~~
$ git diff
~~~
{: .language-bash}

~~~
diff --git a/mars.txt b/mars.txt
index df0654a..315bf3a 100644
--- a/mars.txt
+++ b/mars.txt
@@ -1 +1,2 @@
 Cold and dry, but everything is my favorite color
+The two moons may be a problem for Wolfman
~~~
{: .output}

The `+` marks the added line. To save it we **must `git add` first, then
commit** — Git only commits what you have staged:

~~~
$ git add mars.txt
$ git commit -m "Add concerns about effects of Mars' moons on Wolfman"
~~~
{: .language-bash}

This two-step (stage, then commit) lets you group related changes into one
snapshot.

![The Git Staging Area](../fig/git-staging-area.svg)

`git diff` shows unstaged changes; `git diff --staged` shows what you've staged
but not yet committed. The full add → commit workflow:

![The Git Commit Workflow](../fig/git-committing.svg)

> ## (Optional) Choosing a commit message
>
> Which is the best message for a change adding a line about the climate?
>
> 1. "Changes"
> 2. "Added line 'But the Mummy will appreciate the lack of humidity' to mars.txt"
> 3. "Discuss effects of Mars' climate on the Mummy"
>
> > ## Solution
> > Answer 1 is too vague, 2 is redundant with the diff, **3 is best**: short but
> > descriptive.
> {: .solution}
{: .challenge}

## 5. Exploring and restoring history

Add another line and commit it so we have some history to explore:

~~~
$ nano mars.txt   # add: "But the Mummy will appreciate the lack of humidity"
$ git add mars.txt
$ git commit -m "Discuss concerns about Mars' climate for Mummy"
~~~
{: .language-bash}

`HEAD` refers to the most recent commit. Use `HEAD~1`, `HEAD~2`, … for earlier
ones. Compare the working file to an earlier commit:

~~~
$ git diff HEAD~1 mars.txt
$ git diff HEAD~2 mars.txt
~~~
{: .language-bash}

You can also refer to a commit by its ID (the first few characters are enough),
as shown by `git log`.

Now suppose we wreck the file:

~~~
$ nano mars.txt   # overwrite everything with junk
~~~
{: .language-bash}

Restore the last committed version with `git restore`:

~~~
$ git restore mars.txt
$ cat mars.txt
~~~
{: .language-bash}

~~~
Cold and dry, but everything is my favorite color
The two moons may be a problem for Wolfman
But the Mummy will appreciate the lack of humidity
~~~
{: .output}

`git restore HEAD <file>` restores the last committed version; a commit ID in
place of `HEAD` restores an even older version. To undo the change we want the
commit ID *before* the change, not the one that introduced it.

![Git Checkout](../fig/git-checkout.svg)

## 6. Ignoring things

Some files (build output, data dumps, editor backups) shouldn't be tracked.
Create a couple of dummy files:

~~~
$ mkdir results
$ touch a.dat b.dat c.dat results/a.out
~~~
{: .language-bash}

`git status` clutters up with these. Tell Git to ignore them by creating a
`.gitignore` file:

~~~
$ nano .gitignore
$ cat .gitignore
~~~
{: .language-bash}

~~~
*.dat
results/
~~~
{: .output}

Now `git status` is clean except for `.gitignore` itself — which we **do** want
to commit and share, since collaborators will want to ignore the same things:

~~~
$ git add .gitignore
$ git commit -m "Add the ignore file"
~~~
{: .language-bash}

## 7. Branches: working in parallel

Branches let you work on something without changing `main` (the default "clean"
branch). List branches with `git branch` (the `*` marks your current branch):

~~~
$ git branch
~~~
{: .language-bash}

~~~
* main
~~~
{: .output}

Dracula wants to try an analysis in Python without disturbing `main`. Create a
branch, switch to it, do the work, and commit:

~~~
$ git branch pythondev
$ git switch pythondev
$ touch analysis.py
$ git add analysis.py
$ git commit -m "Wrote and tested python analysis script"
~~~
{: .language-bash}

Switching back to `main`, the file is gone — the work is safely isolated on the
branch:

~~~
$ git switch main
$ ls
~~~
{: .language-bash}

Once we're happy with the work, **merge** it into `main`. First switch to the
branch you're merging *into*, then merge:

~~~
$ git switch main
$ git merge pythondev
~~~
{: .language-bash}

~~~
Updating 12687f6..x792csa1
Fast-forward
 analysis.py | 0
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 analysis.py
~~~
{: .output}

Now the change is in `main`, so we can delete the old branch to avoid confusion.
`-d` refuses to delete unmerged work; `-D` forces it:

~~~
$ git branch -d pythondev
~~~
{: .language-bash}

## 8. Resolving a conflict

A conflict happens when the same lines change on two branches. Let's make one.
Create a new branch and switch to it, and add a line here:

~~~
$ git switch -c marsTemp
$ nano mars.txt   # add: "Yeti will appreciate the cold"
$ git add mars.txt
$ git commit -m "Add a line about the temperature on Mars"
~~~
{: .language-bash}

Now switch to `main` and change **the same last line** differently:

~~~
$ git switch main
$ nano mars.txt   # add: "I'll be able to get 40 extra minutes of beauty rest"
$ git add mars.txt
$ git commit -m "Add a line about the daylight on Mars."
~~~
{: .language-bash}

Merge `marsTemp` into `main` and Git reports a conflict it can't resolve alone:

~~~
$ git switch main
$ git merge marsTemp
~~~
{: .language-bash}

~~~
Auto-merging mars.txt
CONFLICT (content): Merge conflict in mars.txt
Automatic merge failed; fix conflicts and then commit the result.
~~~
{: .output}

![The Conflicting Changes](../fig/conflict.svg)

Git marks both versions in the file:

~~~
...
But the Mummy will appreciate the lack of humidity
<<<<<<< HEAD
I'll be able to get 40 extra minutes of beauty rest
=======
Yeti will appreciate the cold
>>>>>>> 07ebc69c...
~~~
{: .output}

Everything between `<<<<<<< HEAD` and `=======` is your branch's version; between
`=======` and `>>>>>>>` is the incoming version. Edit the file to remove the
markers and keep whatever you want (here, both lines):

~~~
$ nano mars.txt
~~~
{: .language-bash}

~~~
Cold and dry, but everything is my favorite color
The two moons may be a problem for Wolfman
But the Mummy will appreciate the lack of humidity
I'll be able to get 40 extra minutes of beauty rest
Yeti will appreciate the cold
~~~
{: .output}

Then stage and commit to finish the merge:

~~~
$ git add mars.txt
$ git commit -m "Merge changes from marsTemp"
~~~
{: .language-bash}

To reduce conflicts: pull/merge often, keep commits small and focused, and split
large files so collaborators are less likely to edit the same lines.

> ## (Optional) Create and resolve a conflict yourself
>
> - From `main`, create a new branch but stay on `main`.
> - Change a line in `mars.txt` on `main`, then `add` + `commit`.
> - Switch to the new branch, change the **same line**, then `add` + `commit`.
> - Switch back to `main` and `git merge` the new branch.
> - Edit `mars.txt` to remove the `<<<`, `===`, `>>>` markers, then `add` +
>   `commit` to resolve.
{: .challenge}

## Want more detail?

This session condensed the full lessons. For deeper explanations and more
exercises, see:

- [Automated Version Control](../01-basics/)
- [Setting Up Git](../02-setup/) — including SSH (covered in Session 2)
- [Creating a Repository](../03-create/)
- [Tracking Changes](../04-changes/)
- [Exploring History](../05-history/)
- [Ignoring Things](../06-ignore/)
- [Branches](../07-branches/)
- [Conflicts](../08-conflict/)
