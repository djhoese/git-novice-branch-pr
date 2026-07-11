# CLAUDE.md

Guidance for working in this repository.

## What this is

A [Software Carpentry](https://software-carpentry.org/) lesson —
**"Version Control with Git"**, the *branch / pull-request* variant. It is an
alternate version of the standard
[git-novice](https://swcarpentry.github.io/git-novice/) lesson that additionally
teaches **branches** and **merge/pull requests**, and it targets the
**UW-Madison GitLab instance** at `https://git.doit.wisc.edu/` (not GitHub).

- Upstream: <https://github.com/carpentries-incubator/git-novice-branch-pr>
- Rendered site: <https://carpentries-incubator.github.io/git-novice-branch-pr>
- Taught to novice programmers, typically over 4–5 hours.

The running narrative uses Software Carpentry's monster characters
(Dracula, Wolfman, the Mummy, Yeti) planning a base on Mars/Venus, working in a
`planets` repository with a `mars.txt` file.

## Build system

This is a **Jekyll** site built on the Carpentries
[`styles`](https://github.com/carpentries/styles) template.

- `Gemfile` pins the `github-pages` gem (GitHub Pages compatible). Ruby ≥ 2.7.1.
- `Makefile` wraps common commands. Useful targets:
  - `make serve` — build and serve locally (default `http://localhost:4000`).
    Installs gems into `.vendor/bundle` via `bundle`.
  - `make site` — build the site into `_site/` without serving.
  - `make lesson-check` — Carpentries lesson validation.
- Direct equivalent: `bundle install && bundle exec jekyll serve`.
- Generated output goes to `_site/` (git-ignored) — **never hand-edit `_site/`**.
- Deployment: GitHub Pages serves from the **`gh-pages`** branch (this repo's
  default branch).

## Content model

Lesson content is organized as Jekyll **collections** (configured in
`_config.yml`):

- `_episodes/NN-name.md` — the numbered lesson episodes (the main content).
- `_episodes_rmd/` — R-Markdown sources for episodes that are generated (kept in
  sync with `_episodes/`; the "Edit this page" links map between them).
- `_extras/` — supplementary pages (about, discussion, instructor guide, figures).
- `setup.md`, `reference.md`, `index.md`, `aio.md` — landing/setup/reference and
  the all-in-one page.
- `fig/` — figures (SVG diagrams and PNG screenshots) referenced as `../fig/...`.
- `_includes/`, `_layouts/`, `assets/` — Carpentries template machinery; usually
  not edited when authoring content.

### Episode front matter

Each episode is Markdown with YAML front matter:

```yaml
---
title: Tracking Changes
teaching: 20          # minutes of instruction (drives the schedule clock)
exercises: 0          # minutes of exercises (added to the schedule clock)
questions:            # shown in the schedule and episode header
- "How do I record changes in Git?"
objectives:
- "Go through the modify-add-commit cycle."
keypoints:
- "`git add` puts files in the staging area."
---
```

### Carpentries Markdown conventions

- Code/output blocks use `~~~` fences followed by a class attribute line:
  - `{: .language-bash}` (or `{: .bash}`) for commands
  - `{: .output}` for command output
- Special blockquote boxes end with a class attribute:
  - `> ## Title` ... `{: .callout}` — asides/notes
  - `> ## Title` ... `{: .challenge}` — exercises (may contain a nested
    `> > ## Solution` ... `{: .solution}`)
  - `> ## Prerequisites` ... `{: .prereq}`

## Episode ordering (important)

`_config.yml` does **not** define an `episode_order` key. Per
`_includes/manual_episode_order.html`, this means the site falls back to
`lesson_episodes = site.episodes`, which Jekyll sorts **alphanumerically by
filename**.

Consequences:
- Ordering is controlled purely by the numeric filename prefix (`01-`, `02-`, …).
- **Adding a new `_episodes/NN-name.md` file automatically renders it** into the
  schedule table (`_includes/syllabus.html`), the navbar dropdown
  (`_includes/navbar.html`), and the all-in-one page (`aio.md`) — with **no edits
  to any other file**.
- The schedule table shows a single cumulative time clock across all episodes
  (start time from `_config.yml` `start_time`, incremented by each episode's
  `teaching + exercises`).

## Conventions & gotchas

- Keep the monster narrative and the `planets` / `mars.txt` running example
  consistent when editing or adding content.
- Many screenshots in `fig/` are still GitHub-era (`github-*`,
  `github_screenshot_*`); several are commented out (`<!--- ... -->`) in the
  GitLab episodes pending GitLab replacements. Prefer text instructions over
  referencing missing GitLab screenshots.
- Don't commit `_site/` or `.vendor/`.
