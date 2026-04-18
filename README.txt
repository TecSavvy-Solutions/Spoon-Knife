================================================================================
  PHYSIOPLANER – Technische Beschreibung
  Version 1.0 | Stand: April 2026
================================================================================

PROJEKTÜBERSICHT
----------------
PhysioPlaner ist eine webbasierte Einzeldatei-Anwendung für Physiotherapeuten.
Sie ermöglicht das schnelle Erstellen individueller Hausaufgabenpläne mithilfe
einer KI (Google Gemini API) und einer persönlichen Übungsdatenbank.

Keine Installation, kein Server, kein Backend erforderlich.
Die App läuft vollständig im Browser durch einfaches Öffnen der index.html.


DATEIEN
-------
index.html          Komplette Anwendung (HTML + CSS + JavaScript, alles inline)
README.txt          Diese technische Beschreibung
Bedienungsanleitung.txt   Anleitung für Endbenutzer


TECHNOLOGIE-STACK
-----------------
- Sprache:      Vanilla HTML5 / CSS3 / JavaScript (ES2022, kein Framework)
- KI-API:       Google Gemini 1.5 Flash
                https://ai.google.dev/
                Endpunkt: generativelanguage.googleapis.com/v1beta/models/
                          gemini-1.5-flash:generateContent
- Word-Export:  docx.js v8.5.0 (geladen per CDN von unpkg.com)
                https://docx.js.org/
- Datenspeicher: Browser localStorage (kein externer Dienst, kein Server)
- Deployment:   Statische Datei – GitHub Pages, lokaler Browser oder USB-Stick


ARCHITEKTUR
-----------
Alle Logik befindet sich in einer einzigen Datei (index.html):

  ┌─────────────────────────────────────────────────────┐
  │                    index.html                       │
  │                                                     │
  │  ┌──────────┐  ┌──────────────────┐                 │
  │  │  Tab 1   │  │     Tab 2        │                 │
  │  │  Plan    │  │  Übungsdatenbank │                 │
  │  │ erstellen│  │  (CRUD + Backup) │                 │
  │  └────┬─────┘  └────────┬─────────┘                 │
  │       │                 │                           │
  │  ┌────▼─────────────────▼─────────────────────┐    │
  │  │            JavaScript-Logik                 │    │
  │  │  generatePlan()   exportWord()              │    │
  │  │  saveExercise()   backupBrain()             │    │
  │  │  importBrain()    renderBrainTable()        │    │
  │  └────┬────────────────────────────────────────┘    │
  │       │                                             │
  │  ┌────▼───────────────┐  ┌────────────────────┐    │
  │  │  localStorage      │  │   Gemini API       │    │
  │  │  physio_brain      │  │   (Internet)       │    │
  │  │  physio_api_key    │  │                    │    │
  │  └────────────────────┘  └────────────────────┘    │
  └─────────────────────────────────────────────────────┘


DATENSPEICHERUNG
----------------
Alle Daten verbleiben ausschließlich lokal im Browser:

  localStorage["physio_brain"]
    JSON-Array der Übungsdatenbank.
    Format je Eintrag:
    {
      "id":   "uuid-v4-string",
      "name": "Brücke (Gluteal Bridge)",
      "cat":  "Kräftigung",          // Warm-up | Lockerung | Kräftigung
      "desc": "Ausführungsbeschreibung..."
    }

  localStorage["physio_api_key"]
    Gemini API Key als Klartext-String.
    Wird nur lokal gespeichert, nie an Dritte übertragen.

HINWEIS: localStorage ist browserspezifisch. Bei Browserwechsel oder
Gerätewechsel muss die Datenbank per Backup-Funktion übertragen werden.


BACKUP-FORMAT
-------------
Der Export (Schaltfläche "↓ Backup") erzeugt eine JSON-Datei:
  physio_datenbank_TT-MM-JJJJ.json

Die Datei enthält das vollständige Array aller Übungseinträge und kann
über "↑ Importieren" in jeden Browser/jedes Gerät zurückgespielt werden.
Beim Import kann gewählt werden zwischen:
  - Zusammenführen (bestehende + neue Einträge)
  - Ersetzen (nur importierte Einträge)
Doppelte IDs werden automatisch neu vergeben.


SICHERHEITSMERKMALE
-------------------
- API Key wird nie in den Quellcode geschrieben; ausschließlich per
  Einstellungs-Modal in localStorage gespeichert.
- Benutzereingaben werden vor der Prompt-Einbettung sanitized:
  Sonderzeichen (Backticks, Backslashes) werden entfernt,
  maximale Eingabelänge: 500 Zeichen pro Feld.
- Kein externes Tracking, keine Analytics, keine Cookies.
- docx.js wird per CDN geladen (unpkg.com) – bei Bedarf kann die
  Bibliothek auch lokal eingebunden werden (Datei herunterladen und
  <script src="..."> anpassen).


INTERNETVERBINDUNG
------------------
Benötigt für:
  - Gemini API-Aufrufe (Plangenerierung)
  - docx.js laden beim ersten Start (danach ggf. gecacht)

Nicht benötigt für:
  - Übungsdatenbank verwalten (CRUD)
  - Backup herunterladen / importieren


ERSTEINRICHTUNG (IT-Übergabe)
------------------------------
1. index.html auf den Zielrechner kopieren (USB, E-Mail, Cloud-Link)
2. Datei im Browser öffnen (Doppelklick oder Rechtsklick → Öffnen mit)
3. Zahnrad-Symbol (oben rechts) → Gemini API Key eintragen → Speichern
4. Fertig. Die Therapeutin kann sofort loslegen.

Gemini API Key besorgen:
  https://aistudio.google.com/app/apikey
  (kostenloser Zugang für moderate Nutzung verfügbar)


BROWSERKOMPATIBILITÄT
---------------------
Getestet und unterstützt:
  Chrome 90+, Edge 90+, Firefox 88+, Safari 15+

Mindestvoraussetzungen:
  - localStorage-Unterstützung
  - Fetch API
  - crypto.randomUUID()
  - ES2022 (async/await, optional chaining)


GIT-REPOSITORY
--------------
Repository:  TecSavvy-Solutions/Spoon-Knife
Branch:      claude/physio-homework-plan-ai-lCRJ1
Hauptdatei:  index.html


ABHÄNGIGKEITEN (extern)
-----------------------
  docx@8.5.0    https://unpkg.com/docx@8.5.0/build/index.js
  Gemini API    https://generativelanguage.googleapis.com


ERWEITERUNGSMÖGLICHKEITEN
--------------------------
- Mehrere Therapeuten: Separate Backup-Dateien pro Person
- Weitere Kategorien: "cat"-Werte in JS und Modal-Select erweitern
- Andere KI: API-URL und Prompt in generatePlan() anpassen
- Offline-Betrieb: docx.js lokal einbinden, Offline-Fallback für API

================================================================================
