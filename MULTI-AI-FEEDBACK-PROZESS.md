# Multi-AI Feedback-Prozess

**Version:** 1.0
**Datum:** 2025-12-30
**Erstellt von:** Claude (Code)

---

## Übersicht

Der Multi-AI Feedback-Prozess ist ein strukturierter Ansatz, bei dem mehrere KI-Assistenten parallel Feedback zu einer Fragestellung geben. Die Ergebnisse werden dann synthesiert und zu einer Entscheidung zusammengeführt.

**Warum?**
- Verschiedene KIs haben unterschiedliche Stärken und Perspektiven
- Konsens mehrerer KIs erhöht Vertrauen in Entscheidungen
- Unterschiedliche Meinungen werden explizit sichtbar
- Dokumentation aller Überlegungen für spätere Referenz

---

## Die Rollen

| Rolle | KI | Stärke |
|-------|-----|--------|
| **Lead/Koordinator** | Claude (Code) | Prompts erstellen, Synthesen schreiben, Entscheidungen vorbereiten |
| **Feedback-Geber** | Claude (Webchat) | Tiefgehende Analyse, Code-Perspektive |
| **Feedback-Geber** | Gemini | Architektur, kritisches Denken |
| **Feedback-Geber** | ChatGPT | UX/UI-Perspektive, Nutzerfreundlichkeit |
| **Feedback-Geber** | Perplexity | Business-Kontext, Recherche, Marktperspektive |
| **Code-Review** | Cursor | Detailliertes Code-Review |
| **Code-Review** | Antigravity | Zweite Code-Review-Perspektive |
| **Entscheider** | Mathias | Finale Entscheidung, Router zwischen KIs |

---

## Der Prozess

### Phase 1: Prompt erstellen

**Verantwortlich:** Claude (Code)

1. **Kontext sammeln**
   - Aktuellen Projektstand verstehen
   - Relevante Dokumente identifizieren
   - Die konkrete Fragestellung definieren

2. **Self-contained Prompt schreiben**
   Der Prompt sollte enthalten:
   - Projektvision (kurz)
   - Aktueller Stand (was funktioniert, was nicht)
   - Datenmodell (wenn relevant)
   - Die konkrete Frage
   - Kandidaten/Optionen zur Bewertung
   - Gewünschtes Antwortformat

3. **Prompt speichern**
   ```
   feedback/runde[N]-[thema]/PROMPT.md
   ```

---

### Phase 2: Feedback sammeln

**Verantwortlich:** Mathias (als "Router")

1. **Prompt an jede KI senden**
   - Claude (Webchat)
   - Gemini
   - ChatGPT
   - Perplexity

2. **Antworten speichern**
   ```
   feedback/runde[N]-[thema]/
   ├── PROMPT.md
   ├── claude.md
   ├── gemini.md
   ├── chatgpt.md
   └── perplexity.md
   ```

3. **Format der Antwort-Dateien**
   ```markdown
   # Feedback: [AI-Name]
   **Datum:** YYYY-MM-DD
   **Runde:** [N] - [Thema]

   [Kopierte Antwort der KI]
   ```

---

### Phase 3: Synthese erstellen

**Verantwortlich:** Claude (Code)

1. **Alle Feedbacks analysieren**
   - Was ist Konsens? (alle einig)
   - Was sind Mehrheitsmeinungen?
   - Wo gibt es Unterschiede?

2. **Synthese-Dokument schreiben**
   ```
   docs/decisions/YYYY-MM-DD-[thema]-synthese.md
   ```

3. **Format der Synthese**
   ```markdown
   # Feedback-Synthese: Runde [N] - [Thema]

   **Datum:** YYYY-MM-DD
   **Erstellt von:** Claude (Code)
   **Input von:** [Liste der KIs]

   ## Zusammenfassung
   [Kurze Zusammenfassung]

   ## Konsens (alle einig)
   [Was alle KIs gleich sehen]

   ## Mehrheit (3-4 von 4)
   [Was die meisten sehen]

   ## Unterschiedliche Meinungen
   | Punkt | Claude | Gemini | ChatGPT | Perplexity |
   |-------|--------|--------|---------|------------|
   | ... | ... | ... | ... | ... |

   ## Empfehlung
   [Konkrete Empfehlung basierend auf Synthese]

   ## Offene Entscheidungen
   [Was Mathias entscheiden muss]
   ```

---

### Phase 4: Entscheidung

**Verantwortlich:** Mathias

1. **Synthese lesen**
2. **Bei Unklarheiten:** Rückfragen an Claude (Code) oder neue Runde
3. **Entscheidung treffen** und in Synthese dokumentieren
4. **CONTEXT.md aktualisieren**

---

## Ordnerstruktur

```
/feedback/
├── runde1-features/
│   ├── PROMPT.md
│   ├── claude.md
│   ├── gemini.md
│   ├── chatgpt.md
│   └── perplexity.md
├── runde2-architektur/
│   └── ...
├── runde3-lovable-prompts/
│   └── ...
├── runde4-code-review/
│   ├── PROMPT.md
│   ├── cursor/
│   │   └── [mehrere Review-Dateien]
│   └── antigravity/
│       └── [mehrere Review-Dateien]
└── runde5-feature-priorisierung/
    └── ...

/docs/decisions/
├── 2025-12-29-feedback-synthese.md
├── 2025-12-29-architektur-synthese.md
├── 2025-12-29-lovable-synthese.md
└── ...
```

---

## Prompt-Template

```markdown
# [Projektname] - Feedback Runde [N]: [Thema]

**Datum:** YYYY-MM-DD
**Runde:** [N] - [Thema]
**Ziel:** [Was soll entschieden werden?]

---

## 1. Kontext

[Kurze Projektbeschreibung]
[Aktueller Stand]
[Relevante Constraints]

---

## 2. Die Frage

[Konkret formulierte Frage]

---

## 3. Optionen/Kandidaten

### Option A: [Name]
[Beschreibung]
**Pro:** ...
**Contra:** ...

### Option B: [Name]
...

---

## 4. Deine Aufgabe

1. [Was soll bewertet werden?]
2. [Welches Format wird erwartet?]

**Antwortformat:**
```
[Gewünschtes Format]
```

---

*Erstellt von Claude (Code) am YYYY-MM-DD*
```

---

## Wann welche Runde?

| Situation | Runde starten? | Beteiligte KIs |
|-----------|----------------|----------------|
| Feature-Entscheidungen | Ja | Alle 4 |
| Architektur-Fragen | Ja | Alle 4 |
| User Stories / Prompts | Ja | Alle 4 |
| Code-Review | Ja | Cursor + Antigravity |
| Kleine Bugfixes | Nein | - |
| Klare Anforderungen | Nein | - |
| Dringende Hotfixes | Nein | - |

---

## Best Practices

1. **Prompts self-contained halten**
   - Nicht auf externe Dokumente verweisen die die KI nicht kennt
   - Alle relevanten Infos im Prompt

2. **Konkretes Antwortformat vorgeben**
   - Macht Synthese einfacher
   - Verhindert "Roman"-Antworten

3. **Constraints klar machen**
   - Zeit, Budget, Tech-Stack
   - Hilft KIs bei pragmatischen Empfehlungen

4. **Unterschiedliche Meinungen wertschätzen**
   - Nicht immer ist Konsens das Ziel
   - Unterschiede zeigen Risiken oder Alternativen auf

5. **Entscheidungen dokumentieren**
   - Warum wurde X gewählt?
   - Was waren die Alternativen?
   - Hilft bei späterem Nachvollziehen

---

## Beispiel: Hub Projekt (2025-12-29)

| Runde | Thema | Ergebnis |
|-------|-------|----------|
| 1 | Features & Datenmodell | Konsens: Pinning, RLS, Status vereinfachen |
| 2 | Architektur | Entscheidung: Lovable + Supabase (nicht Electron) |
| 3 | User Stories & Lovable Prompts | Quick-Capture = Must Have, 7 Prompts |
| 4a | Code-Review Kriterien | CURSOR-REVIEW-SCHEMA.md als Template |
| 4b | Code-Review Durchführung | Cursor + Antigravity Reviews |
| 4c | Code-Review Synthese | P0+P1 Issues identifiziert |
| 4d | Fix-Prompt | CURSOR-FIX-PROMPT.md erstellt |
| 5 | Feature-Priorisierung | [In Arbeit] |

**Ergebnis:** MVP in einem Tag - Konzept → Code → Review → Fixes → Live

---

## Änderungshistorie

| Datum | Änderung | Version |
|-------|----------|---------|
| 2025-12-30 | Initial erstellt | 1.0 |

---

*Dieser Prozess hat sich im AppWerkstatt Hub Projekt bewährt und kann für zukünftige Projekte wiederverwendet werden.*
