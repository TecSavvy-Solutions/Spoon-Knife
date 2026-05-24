# Claude Code – Arbeitsregeln

## Speicher-Workflow

### Mobil (Claude Code Web / App)
- Jedes neue Projekt bekommt ein **eigenes Repository** in der GitHub-Organisation `TecSavvy-Solutions`
- Namenskonvention: `kebab-case` (z.B. `swing-trading-strategy`, `portfolio-tracker`)
- Neue Repos: `public: false` (privat) als Standard, sofern nicht anders gewünscht
- Nach Fertigstellung immer einen **Draft Pull Request** erstellen
- **Temp-Container für neue mobile Projekte:** `TecSavvy-Solutions/claude-projects`

### Desktop (lokale Entwicklung)
- Arbeitsverzeichnis: `C:/homeserver`
- Lokaler Speicher hat Vorrang; kein automatisches GitHub-Push ohne Aufforderung

### Session-Hinweis
- Jede Claude Code Web-Session ist auf das beim Start gewählte Repository beschränkt
- Für ein neues Projekt → neue Session mit dem Ziel-Repository starten
- Migrations-Auftrag im Prompt angeben, z.B. „Migrate aus TecSavvy-Solutions/Spoon-Knife"

## Repositories

| Repository | Zweck | Status |
|---|---|---|
| `TecSavvy-Solutions/Spoon-Knife` | Temporärer Arbeitsbereich (alt) | Abzulösen |
| `TecSavvy-Solutions/swing-trading-strategy` | Swing Trading Pine Script Projekt | Aktiv – Migration ausstehend |
| `TecSavvy-Solutions/claude-projects` | Temp-Container für mobile Projekte | Bereit |

## Aktive Projekte

| Projekt | Ziel-Repository | Typ | Status |
|---|---|---|---|
| Swing Trading Pine Script | `TecSavvy-Solutions/swing-trading-strategy` | Mobil | Migration ausstehend |

## Migration Swing Trading (nächste Session)

Die fertige Strategie liegt in `TecSavvy-Solutions/Spoon-Knife` auf Branch `claude/swing-trading-pine-script-lrtuU`:
- `swing_strategy_htf_htp_pp.pine` – vollständige Pine Script v6 Strategie
- `CLAUDE.md` – diese Datei

**Migrations-Aufgabe:** Datei in `TecSavvy-Solutions/swing-trading-strategy` auf `main` pushen und PR schliessen.
