# FitFlow Booking - Komplettes Projekt-Szenario

**Zweck:** Realistische User Journey durch den AppWerkstatt Hub
**Für:** UX-Review durch Cursor, Antigravity oder andere AI-Tools

---

## Kontext

**Wer bin ich?**
Ich bin Mathias, Solo-Entwickler und Gründer der AppWerkstatt. Ich baue Web-Apps für kleine Unternehmen - von der ersten Idee bis zum Go-Live. Der Hub ist mein zentrales Werkzeug, um Projekte zu dokumentieren und den Überblick zu behalten.

**Das Projekt:**
Lisa Müller betreibt "FitFlow", ein kleines Fitness-Studio-Netzwerk (3 Studios, ~500 Mitglieder). Sie braucht ein Online-Buchungssystem für Kurse. Aktuell läuft alles über Google Sheets und WhatsApp - Chaos pur.

---

## Tag 1: Montag, 6. Januar 2025 - Erstkontakt

### 09:15 - E-Mail von Lisa

Lisa hat mich über meine Website gefunden. Ihre E-Mail:

> "Hallo Herr Wiedmer, wir sind ein kleines Fitness-Studio und brauchen dringend eine bessere Lösung für unsere Kursbuchungen. Aktuell nutzen wir Google Sheets und das ist ein Albtraum. Können Sie uns helfen? Gruß, Lisa Müller, FitFlow GmbH"

### 09:30 - Ich öffne den Hub

**Was ich tun will:**
1. Neues Projekt anlegen
2. Die E-Mail als ersten Logbook-Eintrag dokumentieren
3. Termin für Discovery-Call festhalten

**Erwartete Schritte im Hub:**
1. Dashboard öffnen → "Neues Projekt" Button klicken
2. Projekt-Formular ausfüllen:
   - Name: "FitFlow Booking"
   - Kunde: "Lisa Müller"
   - Firma: "FitFlow GmbH"
   - Status: "Discovery"
   - Phase: "A - Intake"
3. Projekt speichern
4. In das neue Projekt navigieren
5. Logbook-Eintrag erstellen:
   - Typ: "Note"
   - Titel: "Erstanfrage per E-Mail"
   - Inhalt: Die E-Mail zitieren + meine Gedanken
6. Zweiten Logbook-Eintrag:
   - Typ: "Note"
   - Titel: "Discovery-Call vereinbart"
   - Inhalt: "Termin: Mittwoch 8.1., 14:00 Uhr via Google Meet"

**Fragen an den Reviewer:**
- Wie viele Klicks brauche ich vom Dashboard bis zum ersten Logbook-Eintrag?
- Ist klar, wo ich die Kundeninformationen eingebe?
- Kann ich E-Mail-Text einfach reinkopieren (Markdown/Formatierung)?

---

## Tag 3: Mittwoch, 8. Januar 2025 - Discovery Call

### 14:00-14:45 - Call mit Lisa

Während des Calls mache ich mir Notizen (auf Papier oder in einem separaten Dokument). Nach dem Call will ich alles im Hub dokumentieren.

**Was Lisa erzählt hat:**
- 3 Studios in Hamburg (Eppendorf, Eimsbüttel, Winterhude)
- ~500 aktive Mitglieder
- 12 verschiedene Kurstypen (Yoga, Spinning, HIIT, etc.)
- Problem 1: Doppelbuchungen bei beliebten Kursen
- Problem 2: Keine Warteliste
- Problem 3: Trainer wissen nicht, wer kommt
- Budget: "So günstig wie möglich, aber es muss funktionieren"
- Timeline: "Wäre toll, wenn es zum 1. März läuft"
- Sie zeigt mir ihr Google Sheet (Screenshot gemacht)

**Entscheidungen im Call:**
- MVP ohne Warteliste (Lisa: "Können wir später machen")
- Trainer bekommen eigenen Login
- Mitglieder buchen ohne Account (E-Mail-Bestätigung reicht)
- Zahlungen laufen extern (nicht Teil der App)

### 15:00 - Dokumentation im Hub

**Was ich tun will:**
1. Ausführlichen Logbook-Eintrag zum Discovery-Call
2. Screenshot vom Google Sheet als Artefakt hochladen
3. Entscheidungen separat festhalten
4. Budget und Timeline im Projekt aktualisieren

**Erwartete Schritte:**
1. Projekt "FitFlow Booking" öffnen
2. Logbook-Eintrag erstellen:
   - Typ: "Note" oder "Milestone"?
   - Titel: "Discovery Call mit Lisa"
   - Inhalt: Ausführliche Notizen (500+ Wörter)
3. Weiteren Eintrag für Entscheidungen:
   - Typ: "Decision"
   - Titel: "MVP-Scope festgelegt"
   - Inhalt: Was rein kommt, was nicht
4. Artefakt hochladen:
   - Screenshot "google-sheet-chaos.png"
   - Beschreibung: "Aktuelles Buchungssystem (Google Sheet)"
5. Projekt bearbeiten:
   - Budget: 2.800€
   - Zieldatum: 1. März 2025
   - Status: "Development" oder noch "Discovery"?

**Fragen an den Reviewer:**
- Welchen Entry-Type nutze ich für einen Discovery-Call? Gibt es Guidance?
- Kann ich während des Tippens speichern (Auto-Save) oder verliere ich alles bei Browser-Crash?
- Wie lade ich einen Screenshot hoch? Drag & Drop?
- Kann ich das Budget nachtragen ohne das ganze Projekt neu anzulegen?
- Ist klar, wann ich von Phase A zu Phase B wechsle?

---

## Tag 4: Donnerstag, 9. Januar 2025 - Angebot & Zusage

### 10:00 - Angebot erstellen

Ich erstelle das Angebot in einem separaten Tool (Google Docs/PDF). Aber ich will es im Hub dokumentieren.

**Was ich tun will:**
1. Angebot als Artefakt verlinken oder hochladen
2. Logbook-Eintrag: "Angebot versendet"

### 16:30 - Lisa sagt zu!

Lisa hat per E-Mail zugesagt. Projekt startet offiziell.

**Was ich tun will:**
1. Logbook-Eintrag: "Projekt bestätigt!"
2. Status/Phase aktualisieren: Discovery → Development, Phase A → B
3. GitHub Repository anlegen und verknüpfen

**Erwartete Schritte:**
1. Logbook → Neuer Eintrag:
   - Typ: "Milestone"
   - Titel: "Projekt bestätigt - Kickoff!"
   - Inhalt: "Lisa hat zugesagt. Budget 2.800€, Start sofort, Ziel 1. März."
2. Projekt bearbeiten:
   - Status: "Development"
   - Phase: "B - Development"
3. GitHub-Repo erstellen (extern auf GitHub)
4. Im Hub: Repo verknüpfen
   - URL: https://github.com/appwerkstatt-dev/fitflow-booking

**Fragen an den Reviewer:**
- Wie verknüpfe ich ein GitHub-Repo? Wo ist das Eingabefeld?
- Sehe ich sofort den Repo-Status oder muss ich was laden?
- Was passiert wenn ich versehentlich Development-Status setze aber noch in Phase A bin?

---

## Tag 5-10: Entwicklungsphase

### Tägliche Arbeit

Ich arbeite jeden Tag 2-4 Stunden am Projekt. Dabei nutze ich:
- Lovable für die UI
- Cursor für Code-Anpassungen
- Supabase für Backend
- Den Hub für Dokumentation

**Typischer Tagesablauf:**
1. Hub öffnen → FitFlow Projekt
2. Kurz checken: Was war gestern? (Logbook lesen)
3. GitHub-Status laden (wenn implementiert)
4. Arbeiten...
5. Am Ende: Logbook-Eintrag mit Fortschritt

### Tag 7: Montag, 13. Januar - Auth fertig

**Was ich dokumentieren will:**
- Logbook: "Auth implementiert - Magic Link funktioniert"
- Typ: Milestone
- Vielleicht Screenshot vom Login-Screen

### Tag 9: Mittwoch, 15. Januar - Problem!

Es gibt ein Problem mit der Supabase Row-Level-Security. Ich stecke fest.

**Was ich dokumentieren will:**
- Logbook-Eintrag:
  - Typ: "Blocker"
  - Titel: "RLS-Problem bei Kursbuchungen"
  - Inhalt: Technische Details, was ich schon probiert habe
- Vielleicht: Link zu Stack Overflow Frage

**Fragen an den Reviewer:**
- Gibt es einen "Blocker" Entry-Type?
- Kann ich den Eintrag als "wichtig" markieren (Pinning)?
- Wenn ich den Hub morgen öffne, sehe ich sofort dass es einen offenen Blocker gibt?

### Tag 10: Donnerstag, 16. Januar - Problem gelöst

Das RLS-Problem ist gelöst (dank Claude-Hilfe).

**Was ich dokumentieren will:**
- Logbook: Blocker als gelöst markieren? Oder neuen Eintrag?
- Lösung dokumentieren für die Zukunft

**Fragen an den Reviewer:**
- Kann ich einen Blocker-Eintrag als "erledigt" markieren?
- Oder mache ich einen neuen Eintrag "Blocker gelöst"?
- Gibt es eine Verknüpfung zwischen Einträgen?

---

## Tag 15: Dienstag, 21. Januar - Zwischenstand mit Lisa

### 14:00 - Demo-Call

Ich zeige Lisa den aktuellen Stand. Sie ist begeistert, hat aber Änderungswünsche.

**Lisas Feedback:**
- Farben sollen mehr "FitFlow-Grün" sein
- Button-Texte teilweise unklar
- Feature-Request: "Kann man sehen, wie viele Plätze noch frei sind?"

**Was ich dokumentieren will:**
1. Logbook: "Demo-Call - Zwischenstand"
2. Logbook: "Feedback Lisa" mit ihren Punkten
3. Entscheidung: Plätze-Anzeige kommt rein, Farben werden angepasst

**Fragen an den Reviewer:**
- Wie dokumentiere ich Kundenfeedback am besten?
- Sollte es einen Entry-Type "Feedback" geben?
- Kann ich To-Dos aus dem Feedback ableiten (oder ist das outside scope)?

---

## Tag 25: Freitag, 31. Januar - Code Review

### Internes Review

Bevor ich deployed, will ich den Code reviewen lassen (Cursor/Antigravity).

**Was ich tun will:**
1. AI-Export nutzen: "Exportiere Projekt-Context"
2. Den Export an Cursor/Antigravity geben
3. Review-Ergebnisse im Hub dokumentieren

**Erwartete Schritte:**
1. Projekt öffnen
2. "Export für AI" Button finden
3. Context kopieren
4. (Extern: Review durchführen)
5. Logbook: "Code Review durchgeführt"
6. Artefakt: Review-Ergebnis als Datei hochladen

**Fragen an den Reviewer:**
- Wo ist der AI-Export Button?
- Was ist im Export enthalten? (Projektinfo, Logbook, was noch?)
- Ist der Export gut für einen Code-Review oder fehlt was?

---

## Tag 28: Montag, 3. Februar - Deployment

### Go-Live Vorbereitung

Das Projekt ist fertig. Zeit für Deployment.

**Was ich dokumentieren will:**
1. Logbook: "Pre-Deploy Checklist abgearbeitet"
2. Phase wechseln: B → D (Pre-Deploy)
3. Artefakte:
   - SSL-Zertifikat-Info
   - DNS-Einstellungen
   - Backup-Konfiguration

### 16:00 - Live!

Die App ist live unter booking.fitflow.de

**Was ich dokumentieren will:**
1. Logbook: "🚀 GO-LIVE!"
2. Phase: D → E (Deploy)
3. Status: "Development" → "Live"
4. Link zur Live-App als Artefakt

**Fragen an den Reviewer:**
- Kann ich einen Logbook-Eintrag besonders hervorheben (Emoji, Farbe)?
- Gibt es eine "Go-Live Checklist" im Hub?
- Was passiert mit dem Projekt nach Go-Live? Archivieren?

---

## Tag 30+: Betrieb & Support

### Laufender Betrieb

Das Projekt ist live, aber ich muss gelegentlich Support leisten.

**Typische Aktivitäten:**
- Lisa meldet kleinen Bug → Logbook dokumentieren, fixen, dokumentieren
- Monatliches Check-in mit Lisa
- Backup-Überprüfung

**Fragen an den Reviewer:**
- Wie dokumentiere ich laufenden Support?
- Sollte das Projekt in einen "Operate" Status wechseln?
- Wann archiviere ich ein Projekt?

---

## Ende des Szenarios

Nach ~6 Wochen ist FitFlow Booking live und Lisa ist glücklich. Das Projekt bleibt im Hub als Referenz und für zukünftigen Support.

**Gesamte Hub-Nutzung in diesem Projekt:**
- 1 Projekt angelegt
- ~20-30 Logbook-Einträge
- ~5-10 Artefakte (Screenshots, Dokumente, Links)
- 1 GitHub-Repo verknüpft
- Mehrere AI-Exports für Reviews
- Phasenwechsel: A → B → D → E → F

---

## Zusammenfassung: Kritische Momente

| Tag | Moment | Was muss funktionieren |
|-----|--------|------------------------|
| 1 | Erstkontakt | Projekt schnell anlegen, erste Notiz |
| 3 | Discovery | Viel Text eingeben, Screenshot hochladen |
| 4 | Zusage | Status ändern, GitHub verknüpfen |
| 9 | Blocker | Problem dokumentieren, später als gelöst markieren |
| 21 | Demo | Feedback dokumentieren |
| 25 | Review | AI-Export nutzen |
| 28 | Go-Live | Milestone feiern, Status "Live" |
| 30+ | Support | Laufende Dokumentation |

---

*Dieses Szenario dient als Basis für UX-Reviews. Ein Reviewer sollte den Code durchgehen und prüfen, ob jeder dieser Momente gut abgedeckt ist.*
