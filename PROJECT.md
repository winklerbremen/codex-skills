# Globale Codex Skill-Bibliothek

Stand: 2026-06-07

Dieses Repo synchronisiert Tobias Winklers wiederverwendbare Codex-Skills zwischen Rechnern. Es ist projektuebergreifend: Es dient als Skill-Bibliothek, Lernspeicher und Entscheidungshilfe dafuer, welche Skill-Workflows in neuen Projekten geladen werden sollen.

## Codex Quick Context

Fuer neue Chats zuerst `CODEX_START.md` lesen. Diese `PROJECT.md` beschreibt den vollstaendigen Status dieses globalen Skills-Repos.

- GitHub Remote: `https://github.com/winklerbremen/codex-skills.git`
- Lokaler Repo-Pfad: `C:\Users\Anwender\Documents\GitHub\codex-skills`
- Kanonische Skill-Quelle: `C:\Users\Anwender\.codex\skills`
- Branch: `main`
- Wichtige Regel: `CODEX_START.md` plus `PROJECT.md` gilt als Standard fuer kuenftige Projekt-Repos.
- Wichtige Lernregel: Wiederverwendbare neue Erkenntnisse aktiv in passende globale Skills uebernehmen.

## Index

- Zweck: Warum dieses Repo existiert
- Repo-Struktur: Welche Dateien versioniert werden
- Skill-Bibliothek: Wie die globalen Skills projektuebergreifend genutzt werden
- Automatische Learning Updates: Wann Skills aktualisiert werden
- Sync-Workflow: Wie Skills aktualisiert und gepusht werden
- Projektstandard: Welche Dateien neue Projekt-Repos bekommen
- Ausschluesse: Was nicht committed wird
- Aktueller Skill-Bestand: Welche Skill-Ordner enthalten sind

## Zweck

Das Repo dient als privates Backup und Sync-Ziel fuer globale, wiederverwendbare Codex-Skills. Es ist kein normales Kundenprojekt. Es ist die uebergeordnete Bibliothek, mit der kuenftige Chats anhand von Projektart, Stack und Aufgabe entscheiden, welche Skills geladen und befolgt werden sollen.

Projekt-spezifische Fakten gehoeren in das jeweilige Projekt-Repo. Projektuebergreifende Regeln, wiederkehrende Workflows, Stack-Erkenntnisse, QA-Muster und dauerhafte Nutzerpraeferenzen gehoeren in die globalen Skills.

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

## Skill-Bibliothek

Die kanonische Arbeitsquelle fuer Skills ist:

```text
C:\Users\Anwender\.codex\skills
```

Dieses Repo enthaelt die GitHub-synchronisierte Kopie unter:

```text
C:\Users\Anwender\Documents\GitHub\codex-skills\skills
```

Beim Start oder bei der Analyse eines Projekts sollen die passenden Skills anhand von Stack und Aufgabe gewaehlt werden, zum Beispiel WordPress, Etch, Bricks, Automatic.css, Core Framework, Accessibility, GitHub-Sync oder Frontend-Design.

## Automatische Learning Updates

Nach sinnvoll abgeschlossenen Arbeitsbloecken soll eigenstaendig geprueft werden, ob ein neues Learning global wiederverwendbar ist.

In globale Skills aufnehmen, wenn es sich handelt um:

- wiederverwendbare Workflow-Regeln
- Sicherheits-, Backup-, Git- oder Sync-Konventionen
- Stack-Erkennung und Routing zwischen Skills
- builder- oder framework-spezifische Erkenntnisse
- QA-Muster, Debugging-Schritte oder typische Fehlerquellen
- wiederkehrende Nutzerpraeferenzen, die projektuebergreifend gelten

Nicht global aufnehmen:

- reine Projektfacts wie konkrete Page IDs, Template IDs, Farben eines einzelnen Kundenprojekts
- einmalige Chat-Verlaeufe
- offene Projektwuensche, die nur ein bestimmtes Projekt betreffen

Vorgehen bei globalen Learnings:

1. Den engsten passenden Skill auswaehlen.
2. Die kanonische Datei unter `C:\Users\Anwender\.codex\skills` aktualisieren.
3. Die aktualisierte Skill-Datei nach `codex-skills/skills` kopieren.
4. `CODEX_START.md` oder diese `PROJECT.md` nur aktualisieren, wenn sich Struktur, Sync-Workflow oder globales Betriebsmodell aendern.
5. Secret-Scan ausfuehren.
6. Committen und nach GitHub pushen.

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
