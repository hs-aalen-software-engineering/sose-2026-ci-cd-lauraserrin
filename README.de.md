# Refactoring-Übung: Vom Monolithen zu Modulen

> **[DE] Deutsche Version:** Sie lesen sie gerade!
> **[EN] English Version:** Für vollständige Anweisungen auf Englisch, siehe [REFACTORING_GUIDE.md](REFACTORING_GUIDE.md)

Willkommen zur Refactoring-Übung! Diese Aufgabe vertieft die Konzepte aus **Vorlesung 4: Refactoring - Vom Monolithen zu Modulen**.

> **Hinweis:** Fehlermeldungen von automatisierten Prüfungen (GitHub Actions Workflows) erscheinen in **Englisch und Deutsch**.

## 📚 Lernziele

Durch das Abschließen dieser Übung werden Sie:

1. **Den Refactoring-Workflow** aus Vorlesung 4 anwenden, um monolithischen Code in modulare Komponenten zu transformieren
2. **Feature-Branch-Entwicklung** (Vorlesung 3 Teil 1) mit korrektem Git-Workflow verwenden
3. **CI/CD-Praktiken** (Vorlesung 3 Teil 2) mit automatisierten Qualitätsprüfungen befolgen
4. **Inkrementelle Commits** üben, die schrittweise Refactoring-Fortschritte zeigen
5. **An Peer-Code-Reviews teilnehmen**, um voneinander zu lernen

## 🎯 Aufgabenübersicht

Sie werden den monolithischen `src/road_profile_viewer/main.py` (390 Zeilen) in vier fokussierte Module refaktorisieren:

- `geometry.py` - Reine Mathematikfunktionen (Strahlen-Schnittberechnungen)
- `road.py` - Straßenprofil-Generierung
- `visualization.py` - Dash UI-Schicht
- `main.py` - Vereinfachter Einstiegspunkt (~20 Zeilen)

**Das ist genau das, was Sie in Vorlesung 4 gelernt haben!** Folgen Sie der Vorlesung Schritt für Schritt.

## 📋 Anforderungen

### 1. Git-Workflow (25 Punkte)

- [ ] Feature-Branch erstellen: `feature/refactor-to-modules`
- [ ] **Mindestens 3 inkrementelle Commits** durchführen (einen pro Modul-Extraktion)
- [ ] **Aussagekräftige Commit-Nachrichten** schreiben (> 10 Zeichen)
- [ ] Einen **Pull Request** von Ihrem Feature-Branch zu `main` erstellen
- [ ] **NICHT mergen**, bis alle Prüfungen bestanden sind und Sie Peer-Approval haben

### 2. Code-Struktur (35 Punkte)

- [ ] `geometry.py` erstellen mit:
  - `calculate_ray_line()` Funktion
  - `find_intersection()` Funktion
  - Korrekten Docstrings und Type Hints

- [ ] `road.py` erstellen mit:
  - `generate_road_profile()` Funktion
  - Korrekten Docstrings

- [ ] `visualization.py` erstellen mit:
  - `create_dash_app()` Funktion
  - Gesamter Dash UI-Code
  - Imports von `geometry` und `road`

- [ ] `main.py` vereinfachen auf:
  - **Weniger als 30 Zeilen**
  - Nur Imports von `visualization`
  - Enthält nur `main()` Funktion und `if __name__ == '__main__'`

### 3. Code-Qualität (25 Punkte)

- [ ] **Ruff Linting** besteht (keine Stil-Verstöße)
- [ ] **Ruff Formatierung** besteht (Code ist korrekt formatiert)
- [ ] **Pyright** besteht (keine Typ-Fehler)
- [ ] **Korrekte Imports** ohne zirkuläre Abhängigkeiten
- [ ] **Abhängigkeitsfluss**: `main → visualization → geometry/road`

### 4. Peer-Review (15 Punkte)

- [ ] **Review anfordern** von einem Kommilitonen
- [ ] **Approval erhalten** vor dem Mergen
- [ ] **PR eines anderen Studenten reviewen** und konstruktives Feedback geben

## 🚀 Schritt-für-Schritt-Anleitung

### Schritt 1: Repository klonen

```bash
# GitHub Classroom erstellt ein Repo für Sie - klonen Sie es
git clone https://github.com/hs-aalen-software-engineering/refactoring-IHR-USERNAME.git
cd refactoring-IHR-USERNAME
```

### Schritt 2: Feature-Branch erstellen

```bash
# Stellen Sie sicher, dass Sie auf main sind und aktuell
git checkout main
git pull origin main

# Feature-Branch erstellen
git checkout -b feature/refactor-to-modules

# Verifizieren, dass Sie auf dem neuen Branch sind
git branch
```

### Schritt 3: Geometry-Modul extrahieren

**Folgen Sie Vorlesung 4, Abschnitt 6.2**

1. Erstellen Sie `geometry.py` im **src/road_profile_viewer/ Verzeichnis** (gleiche Ebene wie `src/road_profile_viewer/main.py`)
2. Kopieren Sie `calculate_ray_line()` und `find_intersection()` aus `src/road_profile_viewer/main.py`
3. Fügen Sie korrekte Imports hinzu: `import numpy as np`
4. Fügen Sie Type Hints hinzu (siehe Vorlesung 4 Beispiel)
5. Testen Sie, dass es funktioniert (keine Fehler beim Importieren)

**Committen Sie Ihren Fortschritt:**

```bash
git add src/road_profile_viewer/geometry.py
git commit -m "Extract geometry functions to geometry.py

- Move calculate_ray_line() and find_intersection()
- Add type hints and docstrings
- Prepare for modular testing"
```

### Schritt 4: Road-Modul extrahieren

**Folgen Sie Vorlesung 4, Abschnitt 6.3**

1. Erstellen Sie `road.py` im **src/road_profile_viewer/ Verzeichnis**
2. Kopieren Sie `generate_road_profile()` aus `src/road_profile_viewer/main.py`
3. Fügen Sie korrekte Imports und Docstrings hinzu

**Committen Sie Ihren Fortschritt:**

```bash
git add src/road_profile_viewer/road.py
git commit -m "Extract road generation to road.py

- Move generate_road_profile()
- Separate data generation from geometry and UI"
```

### Schritt 5: Visualization-Modul extrahieren

**Folgen Sie Vorlesung 4, Abschnitt 6.4**

1. Erstellen Sie `visualization.py` im **src/road_profile_viewer/ Verzeichnis**
2. Kopieren Sie `create_dash_app()` und den gesamten UI-Code aus `src/road_profile_viewer/main.py`
3. **Fügen Sie Imports mit absoluten Imports hinzu:**
   ```python
   import numpy as np
   import plotly.graph_objects as go
   from dash import Dash, Input, Output, dcc, html

   from road_profile_viewer.geometry import find_intersection
   from road_profile_viewer.road import generate_road_profile
   ```

   **⚠️ WICHTIG:** Verwenden Sie **absolute Imports** (nicht relative Imports mit `.`), um `ImportError: attempted relative import with no known parent package` zu vermeiden, wenn das Modul direkt ausgeführt wird.

   **Warum absolute Imports?** Wenn Sie eine Python-Datei direkt als Skript ausführen, erkennt Python sie nicht als Teil eines Pakets, daher schlagen relative Imports (mit `.`) fehl. Absolute Imports funktionieren immer.

**Committen Sie Ihren Fortschritt:**

```bash
git add src/road_profile_viewer/visualization.py
git commit -m "Extract UI layer to visualization.py

- Move create_dash_app() and all Dash code
- Import from geometry and road modules using absolute imports
- Complete separation of concerns"
```

### Schritt 6: main.py vereinfachen

**Folgen Sie Vorlesung 4, Abschnitt 6.4 (Ende)**

1. Die monolithische `main.py` existiert bereits in `src/road_profile_viewer/`
2. Ersetzen Sie sie mit einer vereinfachten Version (~20 Zeilen):

```python
"""
Road Profile Viewer - Interactive 2D Visualization
===================================================
Main entry point for the road profile viewer application.

This application visualizes a road profile with camera ray intersection
using an interactive Dash interface.
"""

from road_profile_viewer.visualization import create_dash_app


def main():
    """
    Main function to run the Dash application.
    """
    app = create_dash_app()
    print("Starting Road Profile Viewer...")
    print("Open your browser and navigate to: http://127.0.0.1:8050/")
    print("Press Ctrl+C to stop the server.")
    app.run(debug=True)


if __name__ == "__main__":
    main()
```

**⚠️ WICHTIG:** Verwenden Sie **absolute Imports** (`from road_profile_viewer.visualization`) statt relativer Imports (`from .visualization`). Dies ermöglicht es, das Modul direkt mit `python -m road_profile_viewer.main` oder `uv run main.py` ohne Import-Fehler auszuführen.

**Committen Sie Ihren Fortschritt:**

```bash
git add src/road_profile_viewer/main.py
git commit -m "Simplify main.py to entry point only

- Only imports from visualization module using absolute imports
- Acts as entry point (~20 lines)
- Completes refactoring to modular structure"
```

### Schritt 7: Lokale Prüfungen ausführen

**Testen Sie, dass alles funktioniert, bevor Sie pushen!**

```bash
# Abhängigkeiten installieren
uv sync

# Testen, dass die Anwendung ohne Import-Fehler läuft
uv run python -m road_profile_viewer.main

# Server mit Ctrl+C stoppen, dann Qualitätsprüfungen ausführen
uv run ruff check .
uv run ruff format --check .
uv run pyright

# Wenn Sie Fehler sehen, beheben Sie diese und committen Sie die Fixes
```

### Schritt 8: Pushen und Pull Request erstellen

```bash
# Feature-Branch pushen
git push -u origin feature/refactor-to-modules

# PR mit GitHub CLI erstellen (empfohlen)
gh pr create --title "Refactor: Split monolith into focused modules" \
  --body "Refactored src/road_profile_viewer/main.py into modular structure:

- geometry.py: Ray intersection math
- road.py: Road generation
- visualization.py: Dash UI
- main.py: Entry point (~20 lines)

All code quality checks pass.
Ready for review!"

# Oder PR manuell im GitHub-Webinterface erstellen
```

### Schritt 8.1: Branch Protection aktivieren (WICHTIG!)

**⚠️ Bevor Sie fortfahren, schützen Sie Ihren main-Branch, um versehentliche Merges zu verhindern:**

1. Gehen Sie zu Ihrem Repository auf GitHub
2. Klicken Sie auf **Settings** → **Branches** (in der linken Seitenleiste)
3. Unter "Branch protection rules", klicken Sie auf **Add rule**
4. Konfigurieren Sie die Regel:
   - **Branch name pattern**: `main`
   - ✅ **Require a pull request before merging**
   - ✅ **Require status checks to pass before merging**
   - Im Suchfeld, fügen Sie diese erforderlichen Checks hinzu:
     - `check-all-workflows` (Merge Ready Check)
     - `check-structure` (Structure Check)
     - `check-workflow` (Git Workflow Check)
     - `quality` (Code Quality Check)
     - `smoke-tests` (Smoke Tests)
     - `check-review` (Review Check)
   - ✅ **Require branches to be up to date before merging**
5. Klicken Sie auf **Create** (nach unten scrollen)

**Warum das wichtig ist:** Branch Protection stellt sicher, dass Sie **nicht mergen können**, bis alle automatisierten Prüfungen bestanden sind. Dies verhindert, dass fehlerhafter Code in Ihren main-Branch gelangt!

### Schritt 9: Auf CI-Prüfungen warten

**GitHub Actions führt automatisch aus:**

- ✅ Structure Check (verifiziert, dass Dateien existieren, Imports korrekt sind, etc.)
- ✅ Git Workflow Check (verifiziert Feature-Branch, inkrementelle Commits)
- ✅ Code Quality Check (Ruff, Pyright)
- ✅ Smoke Tests (verifiziert, dass grundlegende Funktionalität funktioniert)
- ✅ Review Check (verifiziert Peer-Review-Prozess)
- ✅ Merge Ready Check (wartet, bis alle anderen Prüfungen bestanden sind)

**Prüfen Sie den "Actions"-Tab auf GitHub, um Ergebnisse zu sehen.**

**⚠️ WICHTIG - NICHT MERGEN, BIS ALLE PRÜFUNGEN BESTANDEN SIND!**

Der **Merge Ready Check** überwacht alle anderen Workflows und wird nur grün, wenn:
- Alle Strukturprüfungen bestanden sind
- Alle Code-Qualitätsprüfungen bestanden sind
- Alle Smoke-Tests bestanden sind
- Git-Workflow korrekt ist
- Sie Peer-Approval erhalten haben

**Wenn Sie Branch Protection aktiviert haben (Schritt 8.1), verhindert GitHub automatisch das Mergen, bis alle erforderlichen Prüfungen bestanden sind.** Dies ist ein Sicherheitsmechanismus zur Gewährleistung der Code-Qualität!

Falls Prüfungen fehlschlagen, lesen Sie die Fehlermeldungen, beheben Sie die Probleme, committen und pushen Sie erneut.

### Schritt 10: Peer-Review erhalten

**Teilen Sie Ihren PR-Link mit einem Kommilitonen:**

```
Hey! Kannst du meinen Refactoring-PR reviewen?
https://github.com/hs-aalen-software-engineering/refactoring-IHR-USERNAME/pull/1
```

**Als Reviewer prüfen Sie:**

- [ ] PR ist vom `feature/refactor-to-modules` Branch
- [ ] Mindestens 3 inkrementelle Commits existieren
- [ ] Alle 4 Dateien existieren (`geometry.py`, `road.py`, `visualization.py`, `main.py`)
- [ ] `main.py` ist vereinfacht (< 30 Zeilen)
- [ ] Funktionen sind in korrekten Modulen
- [ ] Imports fließen korrekt (keine zirkulären Abhängigkeiten)
- [ ] Alle CI-Prüfungen bestehen (grüne Häkchen)

**Wie man approved:**

1. Gehen Sie zum PR
2. Klicken Sie auf "Files changed" Tab
3. Reviewen Sie den Code
4. Klicken Sie auf "Review changes" → "Approve" → "Submit review"

### Schritt 11: PR mergen

**Sobald Sie haben:**
- ✅ Alle CI-Prüfungen bestanden (insbesondere **Merge Ready Check** zeigt ✅)
- ✅ Peer-Review-Approval
- ✅ Keine Konflikte mit main-Branch

**Verifizieren Sie den Merge Ready Check:**

Der **Merge Ready Check** Workflow zeigt Ihnen den Status aller erforderlichen Workflows:
- Wenn er "✅ READY TO MERGE!" zeigt - Sie können mergen!
- Wenn er "⏳ NOT READY - Workflows Running" zeigt - warten Sie, bis alle Prüfungen abgeschlossen sind
- Wenn er "❌ NOT READY TO MERGE" zeigt - beheben Sie zuerst fehlgeschlagene Prüfungen

**Mergen Sie Ihren PR:**

```bash
# Mit GitHub CLI
gh pr merge --squash

# Oder klicken Sie auf "Merge pull request" im GitHub-Webinterface
```

**Hinweis:** Wenn Sie Branch Protection aktiviert haben, erlaubt GitHub das Mergen nur, wenn alle erforderlichen Prüfungen bestanden sind. Der "Merge pull request" Button wird bis dahin deaktiviert sein.

**Herzlichen Glückwunsch! Sie haben die Refactoring-Übung abgeschlossen! 🎉**

## 🛠️ Teil 2: Bauen Sie Ihre eigene CI-Pipeline

> **Vertieft Vorlesung 3 Teil 2** (Abschnitte 4.3.6, 5, 6.3, 11.3).
> Diese Aufgaben werden vom Dozenten im Rahmen des Peer-Reviews geprüft — sie werden **nicht** automatisch bewertet.
> Geschätzte Zeit: **30–45 Minuten**.

### Warum dieser Teil existiert

Bisher haben Sie CI nur *konsumiert*: jeder Workflow in `.github/workflows/` wurde für Sie geschrieben, und Sie mussten ihn nur grün bekommen. In einem echten Team müssen Sie CI auch *selbst schreiben*. Die drei kurzen Aufgaben unten lassen Sie genau die GitHub-Actions-Konfigurationsdateien verfassen, die Sie an Tag 1 jedes neuen Projekts einrichten würden.

Sie können diese als zusätzliche Commits auf demselben `feature/refactor-to-modules`-Branch *vor* dem Mergen machen, oder auf einem Folge-Branch `feature/ci-pipeline` *nachdem* der Refactor-PR gemergt wurde.

### Aufgabe 1 — Status-Badge zur README hinzufügen *(Vorlesung Abschn. 11.3)*

Fügen Sie nahe dem Anfang der `README.md` ein Markdown-Badge hinzu, das anzeigt, ob der `Code Quality`-Workflow gerade besteht. Ersetzen Sie `<owner>` und `<repo>` durch die Werte Ihres Repositorys:

```markdown
![Code Quality](https://github.com/<owner>/<repo>/actions/workflows/quality.yml/badge.svg)
```

**Selbstprüfung:** öffnen Sie Ihr Repository auf GitHub — das Badge sollte als **grün** (besteht) oder **rot** (schlägt fehl) angezeigt werden. In einer frischen PR-Vorschau darf das Bild nicht kaputt sein.

**Committen:**

```bash
git add README.md
git commit -m "Add CI status badge to README"
```

### Aufgabe 2 — Dependabot für GitHub Actions aktivieren *(Vorlesung Abschn. 4.3.6)*

Wenn die Actions, von denen Sie abhängen (z. B. `actions/checkout@v4`, `astral-sh/setup-uv@v4`), neue Versionen veröffentlichen, kann Dependabot automatisch Pull Requests öffnen, damit Sie auf unterstützten, sicherheitsgepatchten Versionen bleiben. Erstellen Sie eine neue Datei `.github/dependabot.yml` mit dem Snippet aus der Vorlesung:

```yaml
version: 2
updates:
  - package-ecosystem: "github-actions"
    directory: "/"
    schedule:
      interval: "weekly"
```

**Selbstprüfung:** nach dem Pushen navigieren Sie in Ihrem Repo zu **Insights → Dependency graph → Dependabot**. Die Konfiguration sollte dort erscheinen. (Dependabot kann bis zu 24 Stunden brauchen, um den ersten Scan durchzuführen.)

**Committen:**

```bash
git add .github/dependabot.yml
git commit -m "Enable Dependabot for GitHub Actions versions"
```

### Aufgabe 3 — Eigenen Quality-Workflow schreiben *(Vorlesung Abschn. 5 + 6.3)*

Schreiben Sie jetzt einen Workflow **von Grund auf**. Erstellen Sie `.github/workflows/student-quality.yml`, der bei jedem Pull Request gegen `main` ausgeführt wird und:

1. das Repository auscheckt (`actions/checkout@v4`)
2. `uv` einrichtet (`astral-sh/setup-uv@v4`)
3. das Verzeichnis `~/.cache/uv` **cached**, mit Schlüssel `hashFiles('**/uv.lock')` (`actions/cache@v4`)
4. Abhängigkeiten mit `uv sync` installiert
5. jeden dieser Befehle als separaten, benannten Schritt ausführt:
   - `uv run ruff check .`
   - `uv run ruff format --check .`
   - `uv run pyright`

> ⚠️ Ihr Workflow lebt **neben** dem `quality.yml` des Dozenten. Löschen oder benennen Sie die Dozenten-Workflows **nicht** um — sie bewerten weiterhin Ihr Refactoring.

**Zwei Wege, das umzusetzen:**

- **Manueller Weg** — öffnen Sie Vorlesung 3 Teil 2, Abschnitt 5.1, und kopieren/passen Sie das YAML an.
- **Copilot-Weg** — öffnen Sie Copilot Chat (`Strg+Alt+I` in VS Code) und fügen Sie einen Prompt wie diesen ein:

  ```
  Create a GitHub Actions workflow at .github/workflows/student-quality.yml for a Python
  project that uses uv. It should trigger on pull requests to main, check out the code,
  set up uv with astral-sh/setup-uv@v4, cache ~/.cache/uv keyed on uv.lock, install with
  uv sync, then run ruff check, ruff format --check, and pyright as separate steps.
  ```

  **Prüfen** Sie immer, was Copilot generiert: verifizieren Sie, dass die Action-Versionen aktuell sind und dass der Cache-Schritt den korrekten Pfad verwendet.

**Selbstprüfung:** öffnen Sie einen PR und bestätigen Sie, dass eine neue Prüfung namens `student-quality` (oder welcher Job-ID Sie wählen) in der Checks-Liste des PR auftaucht, läuft und grün wird.

**Committen:**

```bash
git add .github/workflows/student-quality.yml
git commit -m "Add student-authored code quality workflow"
```

### Abgabe

Wenn alle drei Aufgaben committet und gepusht sind, erwähnen Sie sie in Ihrer PR-Beschreibung, damit der Reviewer weiß, worauf zu achten ist, z. B.:

```
Teil-2-Abgaben:
- README-Badge für den Code-Quality-Workflow
- .github/dependabot.yml (wöchentliche Updates für github-actions)
- .github/workflows/student-quality.yml (PR-getriggerte Quality-Pipeline)
```

Ihr Peer-Reviewer (und der Dozent beim Spot-Check) verifiziert:

- ✅ Die Badge-URL zeigt auf den `quality.yml`-Workflow *Ihres* Repositorys.
- ✅ `.github/dependabot.yml` existiert und enthält den `github-actions`-Ecosystem-Eintrag.
- ✅ `student-quality.yml` ist tatsächlich auf dem PR gelaufen und grün geworden.

## 🔍 Wie Sie bewertet werden

### Automatisierte Prüfungen (85 Punkte)

GitHub Actions verifiziert automatisch:

| Prüfung | Punkte | Was geprüft wird |
|---------|--------|------------------|
| **Structure Check** | 35 | Alle Dateien existieren, Funktionen in korrekten Modulen, Imports korrekt |
| **Git Workflow** | 25 | Feature-Branch, 3+ Commits, aussagekräftige Nachrichten |
| **Code Quality** | 25 | Ruff, Pyright bestehen; keine zirkulären Abhängigkeiten |

### Manuelle Prüfung (15 Punkte)

Dozent verifiziert:

- Sie haben Peer-Review-Approval erhalten
- Das Review war substantiell (nicht nur "LGTM")

## ❓ Fehlerbehebung

### "ImportError: attempted relative import with no known parent package"

Dies ist der häufigste Fehler! Er tritt auf, wenn relative Imports (`.module`) in Dateien verwendet werden, die direkt als Skripte ausgeführt werden.

**Problem:**
```python
# ❌ Dies schlägt fehl, wenn main.py direkt ausgeführt wird:
from .visualization import create_dash_app
from .geometry import find_intersection
```

**Lösung:**
```python
# ✅ Verwenden Sie stattdessen absolute Imports:
from road_profile_viewer.visualization import create_dash_app
from road_profile_viewer.geometry import find_intersection
```

**Warum?** Python erkennt relative Imports nur, wenn eine Datei als Teil eines Pakets importiert wird. Wenn Sie eine Datei direkt ausführen (`python main.py` oder `uv run main.py`), weiß Python nicht, dass sie Teil eines Pakets ist.

**Wenden Sie diesen Fix an auf:**
- `src/road_profile_viewer/main.py`: Import vom `visualization` Modul
- `src/road_profile_viewer/visualization.py`: Import von `geometry` und `road` Modulen

**Verifizierung:**
```bash
# Dies sollte ohne Fehler funktionieren:
uv run python -m road_profile_viewer.main
# Sie sollten sehen: "Starting Road Profile Viewer..."
```

### "Import-Fehler beim lokalen Ausführen"

Stellen Sie sicher, dass Sie alle Module im **src/road_profile_viewer/ Verzeichnis** erstellen. Dies ist die korrekte Python-Paketstruktur!

```
✅ Korrekte Struktur:
road-profile-viewer-IHR-USERNAME/
├── README.md
├── pyproject.toml
├── tests/
│   └── test_smoke.py
└── src/
    └── road_profile_viewer/       ← Alle Module kommen HIERHIN
        ├── __init__.py
        ├── geometry.py            ← ERSTELLEN SIE DIES
        ├── road.py                ← ERSTELLEN SIE DIES
        ├── visualization.py       ← ERSTELLEN SIE DIES
        └── main.py                ← VEREINFACHEN SIE DIES (ursprünglich 390 Zeilen → ~20 Zeilen)

❌ Falsche Struktur:
road-profile-viewer-IHR-USERNAME/
├── geometry.py                    ← FALSCH! Nicht in src/
├── road.py                        ← FALSCH! Nicht in src/
└── src/
    └── road_profile_viewer/
        └── main.py
```

### "Ruff-Fehler"

Wenn Sie Ruff-Fehler bekommen, beheben Sie sie! Die monolithische `main.py` folgt bereits PEP 8, aber beim Extrahieren von Code könnten Sie Probleme einführen:

```python
# ❌ Häufige Fehler beim Extrahieren:
def generate_road_profile(num_points=100,x_max=80):  # Fehlende Leerzeichen nach Komma
    y=0.015 * x_norm**3 * x_max                      # Fehlende Leerzeichen um =
    return x,y                                        # Fehlende Leerzeichen nach Komma

# ✅ Korrekt:
def generate_road_profile(num_points=100, x_max=80):
    y = 0.015 * x_norm**3 * x_max
    return x, y
```

**Auto-Fix der meisten Probleme:**
```bash
uv run ruff check --fix .
uv run ruff format .
```

### "Structure-Check schlägt fehl: main.py zu lang"

Ihre `main.py` sollte **~20 Zeilen** sein, nicht 390! Sie sollten eine **neue** `main.py` nur mit dem Einstiegspunkt erstellen, nicht die gesamte ursprüngliche Datei kopieren.

Die vereinfachte `main.py` sollte nur:
1. Vom `visualization` Modul importieren
2. `main()` Funktion definieren
3. `if __name__ == "__main__":` Block haben

Alles andere gehört in andere Module!

### "Zirkuläre Abhängigkeit erkannt"

Stellen Sie sicher, dass Abhängigkeiten in eine Richtung fließen:

```
✅ Korrekt:
main.py → visualization.py → geometry.py, road.py

❌ Falsch:
geometry.py → visualization.py → geometry.py (ZIRKULÄR!)
```

**Regel:** Module niedrigerer Ebene (`geometry.py`, `road.py`) sollten NICHT von Modulen höherer Ebene (`visualization.py`, `main.py`) importieren.

### "Kein Peer-Review-Approval"

Fragen Sie einen Kommilitonen! Teilen Sie Ihren PR-Link im Kurs-Chat oder während der Vorlesung.

Falls Sie Hilfe beim Finden eines Reviewers brauchen, kontaktieren Sie den Dozenten.

### "CI-Prüfungen laufen nicht"

Stellen Sie sicher:
1. Sie haben einen PR erstellt (nicht nur zu main gepusht)
2. Der PR ist von Ihrem Feature-Branch zu `main`
3. Prüfen Sie den "Actions"-Tab auf Fehler

## 📚 Referenz

- **Vorlesung 4**: Vollständiges Refactoring-Tutorial
- **Vorlesung 3 Teil 1**: Feature-Branch-Workflow
- **Vorlesung 3 Teil 2**: CI/CD-Automatisierung
- **Vorlesung 2**: Code-Qualität (PEP 8, Ruff)

## 🆘 Hilfe bekommen

1. **Vorlesung 4 nochmal lesen** - Sie hat Schritt-für-Schritt-Anweisungen!
2. **CI-Fehlermeldungen prüfen** - Sie sagen Ihnen genau, was falsch ist
3. **Im Kurs-Chat fragen** - Helfen Sie einander!
4. **Sprechstunden** - Dozent ist für Fragen verfügbar

## 📝 Schnellreferenz: Vollständige Import-Struktur

So sollten Ihre Imports in jeder Datei aussehen:

**geometry.py:**
```python
import numpy as np
# Keine Imports von anderen Projektmodulen
```

**road.py:**
```python
import numpy as np
# Keine Imports von anderen Projektmodulen
```

**visualization.py:**
```python
import numpy as np
import plotly.graph_objects as go
from dash import Dash, Input, Output, dcc, html

from road_profile_viewer.geometry import find_intersection
from road_profile_viewer.road import generate_road_profile
```

**main.py:**
```python
from road_profile_viewer.visualization import create_dash_app
```

Viel Erfolg! 🚀

---

**Aufgabe erstellt**: 29.10.2025
**Kurs**: Software Engineering - HS Aalen
**Dozent**: Dominik Mueller
**Letzte Aktualisierung**: 29.10.2025 (Import-Struktur korrigiert)
