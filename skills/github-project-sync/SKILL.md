---
name: github-project-sync
description: Use when setting up, syncing, documenting, or pushing project-status repositories to GitHub for Tobias Winkler. Establishes the default GitHub owner, local clone root, repo structure, privacy expectations, token-light handoff files, and what to include or ignore.
---

# GitHub Project Sync

Use this skill whenever the user wants to put a project status, PROJECT.md, CODEX_START.md, skills, or chat handoff material into Git/GitHub, or asks how to sync projects across devices.

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

## Ignore By Default

Do not commit bulky or local-only artifacts unless the user explicitly asks:

- screenshots and visual QA files: `*.png`, `*.jpg`, `*.jpeg`, `*.webp`, `*.gif`
- generated folders and archives: `generated-*`, `*.zip`
- browser/tool runtime folders: `.playwright-mcp/`
- local logos/assets unless specifically selected for versioning
- secrets, tokens, credentials, MCP config, environment files
- plugin cache skills from `.codex\plugins\cache`

## Global Skills Repo

For reusable personal skills, prefer a separate repo:

```text
C:\Users\Anwender\Documents\GitHub\codex-skills
```

Candidate source folder:

```text
C:\Users\Anwender\.codex\skills
```

The global skills repo should also include `CODEX_START.md` and `PROJECT.md` so future chats can understand the sync setup quickly.

Include user-authored or intentionally customized skills. Exclude `.system` and plugin cache skills unless the user explicitly wants an archive copy.

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
