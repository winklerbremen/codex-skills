---
name: github-project-sync
description: Use when setting up, syncing, documenting, or pushing project-status repositories to GitHub for Tobias Winkler. Establishes the default GitHub owner, local clone root, repo structure, privacy expectations, token-light handoff files, global skill-library updates, and what to include or ignore.
---

# GitHub Project Sync

Use this skill whenever the user wants to put a project status, PROJECT.md, CODEX_START.md, skills, reusable learnings, or chat handoff material into Git/GitHub, or asks how to sync projects across devices.

## Defaults

- GitHub owner/account: `winklerbremen`.
- Local GitHub root: `C:\Users\Anwender\Documents\GitHub`.
- Repositories should be private by default.
- Use branch `main` for new repositories.
- Prefer one repository per client/project.
- Use a separate private `codex-skills` repository for reusable global user skills.

## Token-Light Project Memory

Every maintained project repo should use a two-layer memory model:

1. `CODEX_START.md` is the token-light entry point for new chats.
2. `PROJECT.md` is the full project record with an index and deeper context.

`CODEX_START.md` should stay short enough to read at the beginning of a chat. It should include:

- project name and purpose
- stack, builder, MCP server, key URLs, local path, GitHub remote, branch
- working rules and safety notes
- current status in 5-10 bullets
- the next likely work block
- links to the deeper files and sections

`PROJECT.md` should start with a `Codex Quick Context` section and an `Index`. It stores current truth, important decisions, architecture, IDs, handoffs, QA notes, and meaningful project history. Do not store full chat transcripts unless the user explicitly asks.

When beginning work in a new chat, read `CODEX_START.md` first. Only read `PROJECT.md` sections that are required for the actual task.

## Project Repo Shape

Keep project repos intentionally small:

```text
CODEX_START.md
PROJECT.md
FEATURE_REQUESTS.md
README.md
skills/
.gitignore
```

`CODEX_START.md` is the fast chat handoff and should point to the relevant deeper context.

`PROJECT.md` is the condensed project memory: current state, decisions, URLs, IDs, architecture, handoffs, and important QA notes.

`FEATURE_REQUESTS.md` stores parked ideas or future work that should not clutter the main status.

`skills/` stores only project-relevant skill copies. Do not duplicate every global skill into every project.

## Global Skills Library

`codex-skills` is a cross-project personal skill library, not a normal client/project repo. It should help future chats decide which skill workflow applies to a project based on stack, builder, design system, WordPress setup, frontend needs, accessibility needs, and sync/documentation needs.

Canonical local source:

```text
C:\Users\Anwender\.codex\skills
```

Backup/sync repo:

```text
C:\Users\Anwender\Documents\GitHub\codex-skills
```

GitHub remote:

```text
https://github.com/winklerbremen/codex-skills.git
```

The global skills repo should include `CODEX_START.md` and `PROJECT.md` so future chats can understand the skill-library setup quickly.

Include user-authored or intentionally customized skills. Exclude `.system` and plugin cache skills unless the user explicitly wants an archive copy.

## Learning Update Rule

After meaningful project work, automatically check whether something was learned that should become reusable guidance.

Update global skills when the learning is:

- reusable across multiple projects or clients
- a workflow rule, safety rule, naming rule, QA pattern, stack-detection step, or sync convention
- a correction to an existing skill workflow
- a recurring user preference that should apply beyond the current project
- a builder/framework-specific insight that belongs in an existing stack skill

Keep purely project-specific facts in that project's `PROJECT.md` instead.

When updating global skills:

1. Choose the narrowest existing skill that owns the topic.
2. If no existing skill fits, create or propose a new user-authored skill.
3. Update the canonical skill under `C:\Users\Anwender\.codex\skills` first.
4. Copy the updated skill into `C:\Users\Anwender\Documents\GitHub\codex-skills\skills`.
5. Update `codex-skills/CODEX_START.md` or `PROJECT.md` only when the library structure, sync rule, or global operating model changes.
6. Run a secret scan before committing.
7. Commit and push `codex-skills` so the laptop can pull the updated library.

Do not blindly append every chat detail to global skills. Distill the learning into stable, operational guidance.

## Ignore By Default

Do not commit bulky or local-only artifacts unless the user explicitly asks:

- screenshots and visual QA files: `*.png`, `*.jpg`, `*.jpeg`, `*.webp`, `*.gif`
- generated folders and archives: `generated-*`, `*.zip`
- browser/tool runtime folders: `.playwright-mcp/`
- local logos/assets unless specifically selected for versioning
- secrets, tokens, credentials, MCP config, environment files
- plugin cache skills from `.codex\plugins\cache`

## Setup Workflow

1. Inspect `git status`, `git remote -v`, and candidate files.
2. Create or update `.gitignore` before staging.
3. Create or update `CODEX_START.md` and ensure `PROJECT.md` has `Codex Quick Context` plus `Index`.
4. Stage only important status/skill/docs files.
5. Commit locally with a clear message.
6. If GitHub CLI is unavailable, give the user the GitHub remote commands.
7. If a remote exists, pull before pushing when the repo may already have remote commits.

Typical commands:

```powershell
git remote add origin https://github.com/winklerbremen/<repo-name>.git
git push -u origin main
```

On the laptop or other devices:

```powershell
cd C:\Users\Anwender\Documents\GitHub
git clone https://github.com/winklerbremen/<repo-name>.git
```

For ongoing sync: commit and push from the active machine, then pull on the laptop.
