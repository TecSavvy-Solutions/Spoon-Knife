# PhysioPlaner – Session-Kontext für Claude Code

## Worum geht es

PhysioPlaner ist eine **Single-File-Webanwendung** für Physiotherapeuten.
Die App läuft komplett im Browser (kein Server, keine Installation) – einfach
`index.html` doppelklicken. Sie nutzt die **Google Gemini 1.5 Flash API**, um
aus Patientendaten und einer persönlichen Übungsdatenbank strukturierte
Hausaufgabenpläne zu generieren und als Word-Datei (.docx) zu exportieren.

---

## Status

Die App ist **fertig entwickelt** und funktionsfähig. Alle Dateien liegen auf:

- **Arbeits-Repo (alt):** `TecSavvy-Solutions/Spoon-Knife`
  Branch: `claude/physio-homework-plan-ai-lCRJ1`
  Dateien: `index.html`, `README.txt`, `Bedienungsanleitung.txt`, `CLAUDE.md`

- **Ziel-Repo (neu):** `TecSavvy-Solutions/physioplaner`
  → Hierhin sollen alle Dateien übertragen werden (Hauptzweck dieser Session)

---

## Was in dieser Session zu tun ist

1. Dateien von `Spoon-Knife/claude/physio-homework-plan-ai-lCRJ1` lesen
2. Alle Dateien in `TecSavvy-Solutions/physioplaner` pushen (main branch)
3. `README.txt` anpassen: Repository-Referenz von Spoon-Knife → physioplaner

---

## Technischer Stack

| Komponente | Detail |
|---|---|
| Frontend | Vanilla HTML5 / CSS3 / JS (ES2022), kein Framework |
| KI-API | Google Gemini 1.5 Flash (`generativelanguage.googleapis.com`) |
| Word-Export | docx.js v8.5.0 via CDN (`unpkg.com/docx@8.5.0/build/index.js`) |
| Datenspeicher | Browser `localStorage` – kein Server, kein Backend |
| Deployment | Statische Datei – GitHub Pages oder lokaler Browser |

---

## Architektur (alles in `index.html`)

```
index.html
├── Tab 1: "Plan erstellen"
│   ├── Felder: Patient, Diagnose/Ziel, Übungen (kommagetrennt)
│   ├── Felder: Sätze (default 3), Wiederholungen (default 15)
│   ├── Sprachauswahl (16 Sprachen: DE, EN, TR, AR, RU, PL, FR, ES, IT, HR, SR, RO, PT, EL, SQ, FA)
│   ├── Button "Plan generieren" → Gemini API Call
│   └── Button "Als Word exportieren (.docx)"
│
├── Tab 2: "Übungsdatenbank"
│   ├── CRUD: Hinzufügen / Bearbeiten / Löschen
│   ├── Suchfeld (Echtzeit-Filter)
│   ├── Kategorien: Warm-up | Lockerung / Beweglichkeit | Kräftigung
│   └── Backup (↓ JSON-Export) / Importieren (↑ JSON-Import, 3-Button-Modal)
│
├── Settings-Modal (⚙ Zahnrad)
│   ├── Gemini API Key (gespeichert in localStorage["physio_api_key"])
│   └── Therapeutenname (gespeichert in localStorage["physio_therapist_name"])
│
└── Erster Start: Medizinischer Disclaimer (einmalig wegklicken)
```

---

## localStorage-Schlüssel

| Key | Inhalt |
|---|---|
| `physio_brain` | JSON-Array der Übungsdatenbank |
| `physio_api_key` | Gemini API Key (Klartext, nur lokal) |
| `physio_therapist_name` | Name der Therapeutin/des Therapeuten |
| `physio_disclaimer_v1` | `"accepted"` nach erstem Bestätigen |

---

## Datenmodell Übung

```json
{
  "id": "uuid-v4",
  "name": "Brücke (Gluteal Bridge)",
  "cat": "Kräftigung",
  "desc": "Rückenlage, Füße hüftbreit..."
}
```

---

## Wichtige Implementierungsdetails

- **Gemini API:** `temperature: 0.2` für konsistente medizinische Ausgabe
- **Input-Sanitierung:** `s => String(s).replace(/[`\\]/g, '').substring(0, 500).trim()`
- **Ausgabe-Rendering:** Markdown-ähnlich (##, **, Bullet `-`/`*`) → HTML + docx
- **Bold-in-Bullet:** Regex `^\*\*(.+?)\*\*\s*(.*)` → separate TextRun-Objekte in docx
- **Escape-Key** schließt alle offenen Modals
- **Favicon:** Inline SVG (blaues medizinisches Kreuz, `#1a6fb5`)
- **Import-Modal:** 3 Buttons (Hinzufügen / Ersetzen / Abbrechen) – kein `confirm()`

---

## Word-Export Struktur

1. Titel: "Physiotherapeutische Hausaufgaben"
2. Patient + Datum
3. Therapeutenname (falls gesetzt)
4. Diagnose/Ziel
5. **Allgemeine Tipps** (fixe Section mit Aufbau, Schmerzgrenze, Ausführung, Tempo)
6. Übungsabschnitte (Warm-up → Lockerung → Kräftigung)

---

## Dateien im Repo

| Datei | Beschreibung |
|---|---|
| `index.html` | Komplette App (~900 Zeilen, alles inline) |
| `README.txt` | Technische Beschreibung (Architektur, Sicherheit, Setup) |
| `Bedienungsanleitung.txt` | Endbenutzer-Anleitung (Deutsch, nicht-technisch) |
| `CLAUDE.md` | Diese Datei |

---

## Hintergrund

Die App wurde für eine Physiotherapeutin entwickelt, die:
- Schnell individuelle Hausaufgabenpläne erstellen möchte
- Eine wachsende Übungsdatenbank mit eigenen Beschreibungen pflegt
- Die App lokal auf Windows nutzt (kein Server, kein IT-Aufwand)
- Pläne direkt druckfertig als Word-Datei bekommt
- Patienten in verschiedenen Sprachen betreut (Sprachauswahl im Formular)
