# Globales Codex Skills Backup

Stand: 2026-06-07

Dieses Repo synchronisiert Tobias Winklers wiederverwendbare Codex-Skills zwischen Rechnern. Es ist bewusst schlank gehalten und enthaelt keine System-Skills, Plugin-Cache-Skills, Secrets oder lokalen Codex-Konfigurationen.

## Codex Quick Context

Fuer neue Chats zuerst `CODEX_START.md` lesen. Diese `PROJECT.md` beschreibt den vollstaendigen Status dieses globalen Skills-Repos.

- GitHub Remote: `https://github.com/winklerbremen/codex-skills.git`
- Lokaler Repo-Pfad: `C:\Users\Anwender\Documents\GitHub\codex-skills`
- Kanonische Skill-Quelle: `C:\Users\Anwender\.codex\skills`
- Branch: `main`
- Wichtige Regel: `CODEX_START.md` plus `PROJECT.md` gilt als Standard fuer kuenftige Projekt-Repos.

## Index

- Zweck: Warum dieses Repo existiert
- Repo-Struktur: Welche Dateien versioniert werden
- Sync-Workflow: Wie Skills aktualisiert und gepusht werden
- Projektstandard: Welche Dateien neue Projekt-Repos bekommen
- Ausschluesse: Was nicht committed wird
- Aktueller Skill-Bestand: Welche Skill-Ordner enthalten sind

## Zweck

Das Repo dient als privates Backup und Sync-Ziel fuer globale, wiederverwendbare Codex-Skills. Projekt-spezifische Skills duerfen hier liegen, wenn sie bewusst wiederverwendet oder als Referenz gesichert werden sollen. Fuer einzelne Kunden- oder Webseitenprojekte bleiben separate Projekt-Repos sinnvoll.

## Repo-Struktur

```text
CODEX_START.md
PROJECT.md
README.md
.gitignore
skills/
```

- `CODEX_START.md` ist der tokenarme Einstieg fuer neue Chats.
- `PROJECT.md` ist der vollstaendige Status mit Index und Regeln.
- `README.md` erklaert das Repo fuer Menschen.
- `skills/` enthaelt die kopierten Skill-Ordner.

## Sync-Workflow

1. Aenderungen an globalen Skills in `C:\Users\Anwender\.codex\skills` pflegen.
2. Relevante Skill-Ordner nach `C:\Users\Anwender\Documents\GitHub\codex-skills\skills` kopieren.
3. Secret-Scan ausfuehren.
4. `CODEX_START.md`, `PROJECT.md` und README bei Struktur- oder Regel-Aenderungen aktualisieren.
5. Committen und nach GitHub pushen.
6. Auf dem Laptop per `git pull` aktualisieren.

## Projektstandard

Neue Projekt-Repos sollen standardmaessig diese Dateien enthalten:

```text
CODEX_START.md
PROJECT.md
FEATURE_REQUESTS.md
README.md
skills/
.gitignore
```

- `CODEX_START.md`: schneller, tokenarmer Chat-Einstieg mit aktuellem Stand und Links.
- `PROJECT.md`: vollstaendige Projektakte mit Index, Entscheidungen, Status und Handoffs.
- `FEATURE_REQUESTS.md`: geparkte Ideen und spaetere Wuensche.
- `skills/`: nur projektrelevante Skill-Kopien.

## Ausschluesse

Nicht committen:

- `.system` Skills
- Plugin-Cache-Skills aus `.codex\plugins\cache`
- Secrets, Tokens, Passwoerter, MCP-Konfigurationen, lokale Settings
- grosse Screenshots, generierte Assets und temporaere Runtime-Dateien

## Aktueller Skill-Bestand

- `bricksbuilder-automaticcss`
- `bricksbuilder-coreframework`
- `bricks-mobile-first`
- `coreframework`
- `etchwp-automaticcss`
- `frontend-skill`
- `github-project-sync`
- `global-webdesign-learning`
- `novamira-redesign-interne-project`
- `novamira-wordpress-start`
- `semantic-accessibility`
- `webdesign-standards`
- `wordpress-project-intake`
