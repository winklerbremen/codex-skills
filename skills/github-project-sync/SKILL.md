---
name: github-project-sync
description: Use when setting up, syncing, documenting, or pushing project-status repositories to GitHub for Tobias Winkler. Establishes the default GitHub owner, local clone root, repo structure, privacy expectations, and what to include or ignore.
---

# GitHub Project Sync

Use this skill whenever the user wants to put a project status, PROJECT.md, skills, or chat handoff material into Git/GitHub, or asks how to sync projects across devices.

## Defaults

- GitHub owner/account: `winklerbremen`.
- Local GitHub root: `C:\Users\Anwender\Documents\GitHub`.
- Repositories should be private by default.
- Use branch `main` for new repositories.
- Prefer one repository per client/project.
- Use a separate private `codex-skills` repository for reusable global user skills.

## Project Repo Shape

Keep project repos intentionally small:

```text
PROJECT.md
FEATURE_REQUESTS.md
README.md
skills/
.gitignore
```

`PROJECT.md` is the condensed project memory: current state, decisions, URLs, IDs, architecture, handoffs, and important QA notes. Do not store full chat transcripts unless the user explicitly asks.

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

Include user-authored or intentionally customized skills. Exclude `.system` and plugin cache skills unless the user explicitly wants an archive copy.

## Setup Workflow

1. Inspect `git status`, `git remote -v`, and candidate files.
2. Create or update `.gitignore` before staging.
3. Stage only important status/skill/docs files.
4. Commit locally with a clear message.
5. If GitHub CLI is unavailable, give the user the GitHub remote commands.
6. If a remote exists, pull before pushing when the repo may already have remote commits.

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
