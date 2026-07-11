---
title: "Slim Session 2: Collaborating with GitLab"
teaching: 55
exercises: 0
questions:
- "How do I authenticate to GitLab with SSH?"
- "How do I share my changes with others on the web?"
- "How do I suggest changes to a project I don't own using a merge request?"
objectives:
- "Create an SSH key pair and add it to GitLab."
- "Connect a local repository to a GitLab project and push/pull changes."
- "Fork a project, add a remote, and open a merge request."
keypoints:
- "SSH keys let your computer authenticate to GitLab without typing a password each time."
- "`git push` sends local commits to a remote; `git pull` brings remote commits down."
- "`origin` is your own remote; `upstream` is the authoritative project you forked from."
- "A merge request proposes your branch's changes for review before they're merged."
---

> ## About this slim session
>
> This is a condensed, ~1-hour **follow-along** version covering collaboration on
> the [UW-Madison GitLab](https://git.doit.wisc.edu/) instance. It draws from the
> setup episode (SSH) and the full episodes 9–10. Work through the full lessons
> (linked at the end) for more detail.
>
> This session assumes you completed Session 1 (you have Git configured and a
> local `planets` repository). Challenge boxes are **optional** and not counted
> in the timing.
{: .prereq}

## 1. SSH key setup

Before your computer can talk to GitLab, it needs to prove it's you. We use
**SSH keys**: a **private key** that stays on your computer (never share it) and
a **public key** you give to GitLab. Think of the public key as a padlock and
the private key as the only key that opens it.

First, check whether you already have keys:

~~~
$ ls -al ~/.ssh
~~~
{: .language-bash}

If you see `id_ed25519` and `id_ed25519.pub` (or `id_rsa` / `id_rsa.pub`), you
already have a key pair and can skip generating one. Otherwise create one — use
**your own** email:

~~~
$ ssh-keygen -t ed25519 -C "vlad@tran.sylvan.ia"
~~~
{: .language-bash}

Press <kbd>Enter</kbd> to accept the default file location. You may set a
passphrase (recommended on shared computers) — note there is no "reset
password" option, so remember it. The shell shows nothing as you type it.

Now display your **public** key (note the `.pub`!) and copy it:

~~~
$ cat ~/.ssh/id_ed25519.pub
~~~
{: .language-bash}

~~~
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIDmRA3d51X0uu9wXek559gfn6UFNF69yZjChyBIU2qKI vlad@tran.sylvan.ia
~~~
{: .output}

In your browser at <https://git.doit.wisc.edu>, click your profile icon →
**Preferences** → **SSH Keys** → **Add new key**. Paste the key, give it a
title, and click **Add key**.

Test the connection:

~~~
$ ssh -T git@git.doit.wisc.edu
~~~
{: .language-bash}

~~~
Welcome to GitLab, @VLAD.DRACULA!
~~~
{: .output}

That confirms your key works. You only need to do this once per computer.

## 2. Remotes: push and pull with GitLab

A **remote** is a copy of your repository hosted elsewhere — here, on GitLab.
Sharing work means copying commits between your local repo and the remote.

### Create the GitLab project

Log in to [GitLab](https://git.doit.wisc.edu/) → **New project** → **Create
blank project**. Name it `planets`. Choose a visibility level (Private, Internal,
or Public). **Uncheck** "Initialize repository with a README" — we already have a
local repo. Click **Create project**.

GitLab shows an empty project and an SSH address. Click the blue **Code** button
and copy the **Clone with SSH** address.

### Connect local to remote and push

In your local `planets` repository, add the remote (use **your** username):

~~~
$ git remote add origin git@git.doit.wisc.edu:VLAD.DRACULA/planets.git
$ git remote -v
~~~
{: .language-bash}

~~~
origin   git@git.doit.wisc.edu:VLAD.DRACULA/planets.git (fetch)
origin   git@git.doit.wisc.edu:VLAD.DRACULA/planets.git (push)
~~~
{: .output}

`origin` is just the conventional name for your main remote. Now push your
`main` branch up to GitLab:

~~~
$ git push origin main
~~~
{: .language-bash}

Refresh the GitLab page and you'll see your files. You can also pull changes
down (no effect yet, since everything is already in sync):

~~~
$ git pull origin main
~~~
{: .language-bash}

### Syncing between two computers

The real payoff: use GitLab to sync work between machines. Let's pretend a new
folder is a second computer, and **clone** the project onto it:

~~~
$ cd
$ mkdir laptop2
$ cd laptop2
$ git clone git@git.doit.wisc.edu:VLAD.DRACULA/planets.git
$ cd planets
~~~
{: .language-bash}

`git clone` copies the repo, sets up the `origin` remote, and pulls the history —
all in one step. Make a change here, commit it, and push:

~~~
$ nano venus.txt   # "The lack of moons may make this an ideal place for Wolfman"
$ git add venus.txt
$ git commit -m "Notes on Venus' moons"
$ git push origin main
~~~
{: .language-bash}

Back on your first computer, that change isn't there yet — until you **pull** it:

~~~
$ git pull origin main
$ ls
~~~
{: .language-bash}

Best practice when using several computers: `commit` and `push` at the end of a
session, and `pull` before you start on another machine.

> ## (Optional) Push vs. commit
>
> How is `git push` different from `git commit`?
>
> > ## Solution
> > `git commit` records changes in your **local** repository. `git push` sends
> > those local commits to a **remote** (sharing them with others / other
> > machines).
> {: .solution}
{: .challenge}

## 3. Merge requests: suggesting changes

A **merge request** proposes changes to a project without editing it directly —
useful when you don't have write access, or you want your changes reviewed. We'll
work on a shared `countries` project your instructor provides.

### Fork and clone

Open the instructor's `countries` project in GitLab and click **Fork** (top
right). Forking makes **your own copy** of the project under your GitLab account.

Clone **your fork** to your computer (use **your** username):

~~~
$ cd ~/Desktop
$ git clone git@git.doit.wisc.edu:USERNAME/countries.git
$ cd countries
~~~
{: .language-bash}

Cloning sets up `origin` pointing at your fork. Now add a second remote,
`upstream`, pointing at the instructor's authoritative project (copy its SSH
address from the "forked from" link):

~~~
$ git remote add upstream git@git.doit.wisc.edu:INSTRUCTOR-GIVEN/countries.git
$ git remote -v
~~~
{: .language-bash}

~~~
origin    git@git.doit.wisc.edu:USERNAME/countries.git (fetch)
origin    git@git.doit.wisc.edu:USERNAME/countries.git (push)
upstream  git@git.doit.wisc.edu:INSTRUCTOR-GIVEN/countries.git (fetch)
upstream  git@git.doit.wisc.edu:INSTRUCTOR-GIVEN/countries.git (push)
~~~
{: .output}

So: **`origin`** = your fork, **`upstream`** = the original.

### Make changes on a branch

Pull the latest from `upstream`, then update your fork with it:

~~~
$ git pull upstream main
$ git push origin main
~~~
{: .language-bash}

Create a branch named after the country you'll add (pick a unique one), and
switch to it in one step:

~~~
$ git checkout -b addFrance
$ git branch
~~~
{: .language-bash}

Copy an existing country file, edit it for your country, then add and commit:

~~~
$ cp united_states.txt france.txt
$ nano france.txt
$ git add france.txt
$ git commit -m "Added file on france"
~~~
{: .language-bash}

### Push the branch and open the merge request

Push your **branch** to your fork (`origin`):

~~~
$ git push origin addFrance
~~~
{: .language-bash}

Reload your fork in GitLab. It notices the new branch and offers a **Create
merge request** button — click it. Set the source to your fork's `addFrance`
branch and the target to the **upstream** project's `main` branch. Add a title/
description, then **Create merge request**.

Now a maintainer of the upstream project can review, comment, and merge your
changes. If a reviewer asks for a tweak, just make another commit on the same
branch and push it — the merge request updates automatically:

~~~
$ nano france.txt   # e.g. add "Largest City: Paris"
$ git add france.txt
$ git commit -m "Added largest city to france file"
$ git push origin addFrance
~~~
{: .language-bash}

New commits pushed to the same branch join the existing merge request. Use a
**separate branch** if you want changes considered independently.

> ## (Optional) Add another country and make a second MR
>
> - From `main`, make a new branch (`git checkout main` then `git checkout -b addItaly`).
> - Copy another country file, edit it, then `add` + `commit`.
> - Push the branch (`git push origin addItaly`) and open a merge request.
{: .challenge}

## Want more detail?

This session condensed the full lessons. For deeper explanations, screenshots,
and more exercises, see:

- [Setting Up Git](../02-setup/) — full SSH setup, proxies, password managers
- [Remotes in GitLab](../09-gitlab/) — including unrelated-histories pitfalls
- [Merge Requests](../10-merge-requests/)
