# Codex Start

Stand: 2026-06-07

## Schnellkontext

- Projekt: Globale Codex Skill-Bibliothek
- Zweck: Projektuebergreifende Skill-Auswahl, Lernspeicher und Synchronisation zwischen Rechnern
- GitHub Remote: `https://github.com/winklerbremen/codex-skills.git`
- Lokaler Repo-Pfad: `C:\Users\Anwender\Documents\GitHub\codex-skills`
- Kanonische Skill-Quelle: `C:\Users\Anwender\.codex\skills`
- Branch: `main`

## Arbeitsmodus

- Dieses Repo ist projektuebergreifend und gilt als persoenliche Skill-Bibliothek.
- Es hilft kuenftigen Chats zu entscheiden, welcher Skill fuer welchen Stack oder Projekttyp gebraucht wird.
- `.system` und Plugin-Cache-Skills bleiben ausgeschlossen.
- Keine Secrets, Tokens, MCP-Konfigurationen oder lokalen Settings committen.
- Aenderungen zuerst in der kanonischen Skill-Quelle pflegen, dann ins Repo kopieren und pushen.
- Nach relevanten Learnings automatisch pruefen, ob ein globaler Skill aktualisiert werden soll.

## Aktueller Stand

- Globales Skills-Backup ist erstellt und nach GitHub gepusht.
- `github-project-sync` definiert die Standardstruktur fuer Projekt-Repos.
- Neue Projekt-Repos sollen `CODEX_START.md` fuer den schnellen Einstieg und `PROJECT.md` als Vollstatus enthalten.
- Wiederverwendbare neue Erkenntnisse sollen in passende globale Skills destilliert werden, nicht nur in Projektakten.

## Was Wird Regelmaessig Aktualisiert?

- Einzelne Skill-Dateien unter `C:\Users\Anwender\.codex\skills`, wenn ein wiederverwendbares Learning entsteht.
- Die kopierten Skill-Dateien unter `skills/` in diesem Repo.
- `PROJECT.md`, wenn sich Struktur, Regeln, Sync-Workflow oder Skill-Bestand aendern.
- `CODEX_START.md`, wenn der schnelle Einstieg veraltet ist.

## Kontext Bei Bedarf

- Vollstatus und Index: `PROJECT.md`
- Skill-Dateien: `skills/`
- Sync- und Lernregel: `skills/github-project-sync/SKILL.md`
- Kanonischer Sync-Skill: `C:\Users\Anwender\.codex\skills\github-project-sync\SKILL.md`
