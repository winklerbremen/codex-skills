# Codex Start

Stand: 2026-06-07

## Schnellkontext

- Projekt: Globales Codex Skills Backup
- Zweck: Private Synchronisation wiederverwendbarer Codex-Skills zwischen Rechnern
- GitHub Remote: `https://github.com/winklerbremen/codex-skills.git`
- Lokaler Repo-Pfad: `C:\Users\Anwender\Documents\GitHub\codex-skills`
- Kanonische Skill-Quelle: `C:\Users\Anwender\.codex\skills`
- Branch: `main`

## Arbeitsmodus

- Dieses Repo enthaelt nur user-authored oder bewusst angepasste Skills.
- `.system` und Plugin-Cache-Skills bleiben ausgeschlossen.
- Keine Secrets, Tokens, MCP-Konfigurationen oder lokalen Settings committen.
- Aenderungen zuerst in der kanonischen Skill-Quelle pflegen, dann ins Repo kopieren und pushen.

## Aktueller Stand

- Erstes globales Skills-Backup ist erstellt und nach GitHub gepusht.
- `github-project-sync` definiert die Standardstruktur fuer Projekt-Repos.
- Neue Projekt-Repos sollen `CODEX_START.md` fuer den schnellen Einstieg und `PROJECT.md` als Vollstatus enthalten.

## Kontext Bei Bedarf

- Vollstatus und Index: `PROJECT.md`
- Skill-Dateien: `skills/`
- Sync-Regel: `skills/github-project-sync/SKILL.md`
- Kanonischer Skill: `C:\Users\Anwender\.codex\skills\github-project-sync\SKILL.md`
