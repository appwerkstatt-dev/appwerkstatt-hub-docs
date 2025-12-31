# AppWerkstatt Prozess - App-Lifecycle A-H

**Erstellt:** 2025-12-30
**Basis:** Erfahrungen aus dem AppWerkstatt Hub Projekt

---

## Übersicht: Der App-Lifecycle

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                         APP-LIFECYCLE (A-H)                                 │
│                                                                             │
│  ┌─────┐   ┌─────┐   ┌─────┐   ┌─────┐   ┌─────┐   ┌─────┐   ┌─────┐       │
│  │  A  │──▶│  B  │──▶│  C  │──▶│  D  │──▶│  E  │──▶│  F  │──▶│  G  │       │
│  │Intake│   │ Dev │   │Review│  │Pre- │   │Go-  │   │Opera│   │Inci-│       │
│  │     │   │     │   │     │   │Deploy│  │Live │   │te   │   │dent │       │
│  └─────┘   └─────┘   └─────┘   └─────┘   └─────┘   └─────┘   └─────┘       │
│     │         │         │         │         │         │         │          │
│     │         │         │         │         │         │         ▼          │
│     │         │         │         │         │         │      ┌─────┐       │
│     │         │         │         │         │         └─────▶│  H  │       │
│     │         │         │         │         │                │Decom│       │
│     ▼         ▼         ▼         ▼         ▼                └─────┘       │
│  [Multi-AI] [Multi-AI] [Multi-AI] [Checkliste] [Smoke-Test]                │
│  Discovery  Prompts    Reviews    Infra       Monitoring                   │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Durchgängiges Beispiel: FitFlow Booking

**Kunde:** Lisa Müller, FitFlow GmbH (3 Fitnessstudios, 500 Mitglieder)
**Problem:** Kursbuchungen laufen über Google Sheets - Chaos, Überbuchungen, keine Übersicht
**Lösung:** Web-App für Kursbuchungen mit Mitglieder-Login

---

## Phase A: Intake & Acceptance

**Ziel:** Vom ersten Kundenkontakt zur klaren Projektdefinition

### A.1 Erstkontakt & Discovery-Call

**Trigger:** Kunde meldet sich (E-Mail, Empfehlung, Website)

**Input:**
- Kundenanfrage (oft vage: "Wir brauchen eine App für...")

**Aktivitäten:**
1. Discovery-Call durchführen (30-60 Min)
2. Problem verstehen (nicht Lösung!)
3. Stakeholder identifizieren
4. Budget-Rahmen klären
5. Timeline-Erwartungen abfragen

**Output:**
- Discovery-Notizen im Hub-Logbook
- Erste Artefakte (Screenshots vom aktuellen Prozess, etc.)

```
📝 FitFlow Beispiel - Logbook-Eintrag:
────────────────────────────────────
Typ: note
Titel: Discovery-Call mit Lisa
Inhalt:
- 3 Studios in München
- 500 aktive Mitglieder
- Problem: Google Sheet für Kursbuchungen
- Trainer tragen manuell ein → Überbuchungen
- Lisa wünscht: "Einfach wie ein Kalender"
- Budget: ~3.000€
- Timeline: "Bis Februar wäre toll"
```

**Automatisierungspotenzial:**
- [ ] Meeting-Transkription → strukturierte Notizen
- [ ] Automatische Problem-Extraktion aus Gesprächsnotizen
- [ ] Template für Discovery-Fragen

### A.2 Projektbeschreibung erstellen

**Input:** Discovery-Notizen

**Aktivitäten:**
1. Strukturiertes Dokument erstellen
2. Features ableiten (MVP vs. Later)
3. Abgrenzungen definieren
4. Erfolgskriterien festlegen

**Output:**
- Projektbeschreibung (Artefakt im Hub)
- Feature-Liste mit Priorisierung

**Automatisierungspotenzial:**
- [ ] Discovery-Notizen → Projektbeschreibung (AI-Draft)
- [ ] Automatische Feature-Kategorisierung (Must/Should/Nice)

### A.3 Multi-AI Feedback: Features & Datenmodell

**Input:** Projektbeschreibung

**Aktivitäten:**
1. Self-contained Prompt erstellen
2. An 4 AIs senden (Claude, Gemini, ChatGPT, Perplexity)
3. Responses sammeln
4. Synthese erstellen
5. Entscheidungen dokumentieren

**Output:**
- Validiertes Datenmodell
- Bestätigte Feature-Liste
- Entscheidungs-Dokument

```
📝 FitFlow Beispiel - Multi-AI Frage:
────────────────────────────────────
"Für ein Kursbuchungssystem mit 500 Mitgliedern:
- Brauchen wir Wartelisten?
- Stornierungsfristen als Feature oder später?
- Recurring Bookings vs. Einzelbuchungen?"

Konsens: Warteliste = Nice-to-have, Storno = Must-have
```

**Automatisierungspotenzial:**
- [ ] Parallele API-Calls an alle 4 AIs
- [ ] Automatische Konsens-Erkennung
- [ ] Synthese-Generierung

### A.4 Multi-AI Feedback: Architektur

**Input:** Features + technische Constraints

**Aktivitäten:**
1. Architektur-Optionen aufstellen
2. An 4 AIs senden
3. Synthese + Entscheidung

**Output:**
- Tech-Stack Entscheidung
- Architektur-Dokument

```
📝 FitFlow Beispiel - Architektur-Entscheidung:
────────────────────────────────────
Frage: Lovable+Supabase vs. Next.js+Prisma?
Entscheidung: Lovable+Supabase
Begründung: Schneller MVP, Budget-passend, keine komplexe Backend-Logik nötig
```

**Automatisierungspotenzial:**
- [ ] Tech-Stack-Empfehlung basierend auf Features
- [ ] Automatische Trade-off-Matrix

### A.5 Angebot & Acceptance

**Input:** Validierte Projektbeschreibung, Architektur, Budget

**Aktivitäten:**
1. Angebot erstellen
2. Mit Kunde durchgehen
3. A1-Acceptance-Dokument erstellen (beiderseitige Bestätigung)

**Output:**
- A1-Acceptance.md (Artefakt)
- Projekt-Status: "accepted"

```
📝 FitFlow Beispiel - A1-Acceptance:
────────────────────────────────────
✓ Scope: Kursbuchungs-MVP (7 Features)
✓ Budget: 2.800€
✓ Timeline: 4 Wochen
✓ Tech: Lovable + Supabase
✓ Nicht inkludiert: Warteliste, Payment
Unterschrift: Lisa Müller, 2025-01-06
```

**Automatisierungspotenzial:**
- [ ] Angebots-Generator aus Projektbeschreibung
- [ ] A1-Template mit Auto-Fill
- [ ] E-Signatur-Integration

---

## Phase B: Development

**Ziel:** Von der Spezifikation zum funktionierenden Code

### B.1 Infrastruktur Setup

**Input:** Architektur-Entscheidung

**Aktivitäten:**
1. Backend/DB aufsetzen (Supabase)
2. Schema erstellen
3. Auth konfigurieren
4. Git-Repo erstellen
5. CI/CD vorbereiten (Coolify)

**Output:**
- Lauffähige Infrastruktur
- Schema dokumentiert
- Repo verknüpft (Hub-Link)

```
📝 FitFlow Beispiel - Infrastruktur:
────────────────────────────────────
Supabase: fitflow-booking (Frankfurt)
Tabellen: users, courses, bookings, studios
RLS: Mitglieder sehen nur eigene Buchungen
Repo: github.com/appwerkstatt-dev/fitflow-booking
```

**Automatisierungspotenzial:**
- [ ] Schema aus Datenmodell generieren
- [ ] RLS-Policies aus Feature-Beschreibung
- [ ] Supabase-Projekt via API anlegen
- [ ] Coolify-Projekt via API anlegen

### B.2 Prompts erstellen

**Input:** Feature-Liste (priorisiert)

**Aktivitäten:**
1. Features in Prompts übersetzen
2. Reihenfolge festlegen (Abhängigkeiten)
3. Constraints dokumentieren
4. Optional: Multi-AI Feedback auf Prompts

**Output:**
- Nummerierte Prompt-Liste
- Dokumentiert in Hub-Artefakt

```
📝 FitFlow Beispiel - Prompts:
────────────────────────────────────
1. Auth + Landing Page
2. Dashboard (Mitglieder-Sicht)
3. Kursübersicht + Buchung
4. Meine Buchungen
5. Trainer-Ansicht
6. Admin: Kurse verwalten
7. Admin: Mitglieder verwalten
```

**Automatisierungspotenzial:**
- [ ] Feature → Prompt Konvertierung
- [ ] Dependency-Graph generieren
- [ ] Prompt-Templates pro Feature-Typ

### B.3 Build (iterativ)

**Input:** Prompts

**Aktivitäten:**
1. Prompt in Lovable/Cursor ausführen
2. Ergebnis testen
3. Logbook-Eintrag (was wurde gebaut)
4. Commit + Push
5. Nächster Prompt

**Output:**
- Funktionierender Code
- Git-History
- Logbook-Einträge pro Feature

```
📝 FitFlow Beispiel - Build-Log:
────────────────────────────────────
Prompt 3 fertig: Kursübersicht zeigt alle Kurse der Woche,
Buchung mit einem Klick, Bestätigung per Toast.
Commit: abc123 "feat: course booking"
```

**Automatisierungspotenzial:**
- [ ] Automatische Commits nach Prompt
- [ ] Progress-Tracking (X/Y Prompts done)
- [ ] Screenshot nach jedem Prompt

### B.4 Zwischen-Demo (optional)

**Input:** Aktueller Stand

**Aktivitäten:**
1. Demo-Session mit Kunde
2. Feedback sammeln
3. Anpassungen priorisieren

**Output:**
- Feedback im Logbook
- Ggf. neue Prompts

**Automatisierungspotenzial:**
- [ ] Demo-Link generieren
- [ ] Feedback-Formular

---

## Phase C: Review & Quality Gate

**Ziel:** Qualität sicherstellen vor Deployment

### C.1 Code-Review Setup

**Input:** Feature-Liste, Quality-Standards

**Aktivitäten:**
1. Review-Schema definieren (einmalig pro Projekt-Typ)
2. Review-Prompts erstellen

**Output:**
- Review-Prompts (5 Kategorien)

```
📝 Review-Kategorien:
────────────────────────────────────
1. Inventory (Was gibt es?)
2. Security Audit
3. Feature Verification
4. Code Quality
5. Polish & Accessibility
```

**Automatisierungspotenzial:**
- [ ] Review-Schema aus Projekt-Typ ableiten
- [ ] Automatische Checklist-Generierung

### C.2 Multi-Tool Code-Review

**Input:** Aktueller Code, Review-Prompts

**Aktivitäten:**
1. Reviews mit mehreren Tools (Cursor, Antigravity, Claude Code)
2. Findings sammeln
3. Synthese erstellen
4. Priorisieren (P0-P3)

**Output:**
- Strukturierte Findings
- Priorisierte Fix-Liste
- Fix-Prompt

```
📝 FitFlow Beispiel - Review-Findings:
────────────────────────────────────
P0 (Security): Buchungen ohne Auth-Check möglich
P1 (Feature): Doppelbuchung nicht verhindert
P2 (Quality): Console.logs noch im Code
P3 (Polish): Loading-States fehlen
```

**Automatisierungspotenzial:**
- [ ] Parallele Reviews mit mehreren Tools
- [ ] Automatische Finding-Aggregation
- [ ] Severity-Klassifizierung
- [ ] Fix-Prompt-Generierung

### C.3 Fixes implementieren

**Input:** Fix-Prompt

**Aktivitäten:**
1. P0 + P1 fixen (vor Release)
2. Commit + Push
3. Re-Review (wenn nötig)

**Output:**
- Gefixte Issues
- Sauberer Code

**Automatisierungspotenzial:**
- [ ] Automatische Fix-Anwendung (einfache Fixes)
- [ ] Regression-Check

### C.4 Quality Gate Check

**Input:** Code nach Fixes

**Aktivitäten:**
1. Finale Checklist durchgehen
2. Sign-off dokumentieren

**Output:**
- C1-QualityGate.md (Artefakt)
- Status: "ready for deploy"

```
📝 FitFlow Beispiel - Quality Gate:
────────────────────────────────────
✓ Alle P0/P1 gefixt
✓ Auth funktioniert
✓ RLS getestet
✓ Mobile responsive
✓ Keine Console-Errors
Sign-off: 2025-01-20
```

**Automatisierungspotenzial:**
- [ ] Automatische Checklist-Validierung
- [ ] Lighthouse-Score Check
- [ ] Security-Scan

---

## Phase D: Pre-Deploy

**Ziel:** Produktionsumgebung vorbereiten

### D.1 Domain & SSL

**Input:** Projekt-Name, Kunden-Wünsche

**Aktivitäten:**
1. Domain registrieren/konfigurieren
2. DNS eintragen
3. SSL-Zertifikat (automatisch via Coolify/Let's Encrypt)

**Output:**
- Domain live (ohne App)
- SSL funktioniert

```
📝 FitFlow Beispiel:
────────────────────────────────────
Domain: booking.fitflow.de
DNS: CNAME → coolify.appwerkstatt.dev
SSL: Let's Encrypt (auto)
```

**Automatisierungspotenzial:**
- [ ] DNS-Automatisierung (Cloudflare API)
- [ ] Coolify Domain via API

### D.2 Environment & Secrets

**Input:** Tech-Stack, externe Services

**Aktivitäten:**
1. Produktions-Secrets erstellen
2. Environment Variables setzen
3. Externe Services verbinden (falls nötig)

**Output:**
- Secrets sicher gespeichert
- Env-Vars in Coolify

```
📝 FitFlow Beispiel:
────────────────────────────────────
VITE_SUPABASE_URL=https://xxx.supabase.co
VITE_SUPABASE_ANON_KEY=eyJ... (Prod-Key!)
```

**Automatisierungspotenzial:**
- [ ] Secret-Rotation
- [ ] Env-Sync zwischen Staging/Prod

### D.3 Backup & Monitoring

**Input:** Datenbank, Uptime-Anforderungen

**Aktivitäten:**
1. Backup-Strategie definieren (Supabase: automatisch)
2. Monitoring aufsetzen (optional: UptimeRobot, etc.)

**Output:**
- Backup läuft
- Monitoring aktiv

**Automatisierungspotenzial:**
- [ ] Automatisches Monitoring-Setup
- [ ] Alerting-Konfiguration

---

## Phase E: Deploy & Go-Live

**Ziel:** App live schalten

### E.1 Deployment

**Input:** Code (reviewed), Infrastruktur (ready)

**Aktivitäten:**
1. Finaler Deploy triggern
2. Smoke-Test durchführen
3. URLs verifizieren

**Output:**
- App live unter Produktions-Domain

```
📝 FitFlow Beispiel:
────────────────────────────────────
Deploy: 2025-01-25 14:00
URL: https://booking.fitflow.de
Smoke-Test: ✓ Login, ✓ Buchung, ✓ Mobile
```

**Automatisierungspotenzial:**
- [ ] Automated Smoke-Tests
- [ ] Rollback bei Fehler
- [ ] Deploy-Notification

### E.2 Kunden-Übergabe

**Input:** Live-App

**Aktivitäten:**
1. Demo/Schulung mit Kunde
2. Zugangsdaten übergeben
3. Support-Kanal klären

**Output:**
- Kunde kann App nutzen
- E1-GoLive.md (Artefakt)

```
📝 FitFlow Beispiel - E1-GoLive:
────────────────────────────────────
Go-Live: 2025-01-25
URL: https://booking.fitflow.de
Admin-Zugang: lisa@fitflow.de
Support: support@appwerkstatt.dev
Nächste Schritte: 2 Wochen Beobachtung
```

**Automatisierungspotenzial:**
- [ ] Onboarding-E-Mail automatisch
- [ ] Zugangsdaten sicher übermitteln

### E.3 Abrechnung

**Input:** A1-Acceptance (Budget)

**Aktivitäten:**
1. Rechnung erstellen
2. Versenden

**Output:**
- Rechnung (Artefakt)

**Automatisierungspotenzial:**
- [ ] Rechnungs-Generator
- [ ] Integration mit Buchhaltung

---

## Phase F: Operate

**Ziel:** Laufender Betrieb, Support

### F.1 Monitoring & Health-Checks

**Input:** Live-App

**Aktivitäten:**
1. Uptime überwachen
2. Error-Logs prüfen (periodisch)
3. Performance beobachten

**Output:**
- Kontinuierliche Überwachung
- Logbook-Einträge bei Auffälligkeiten

**Automatisierungspotenzial:**
- [ ] Automatische Alerts
- [ ] Weekly Health-Report
- [ ] Log-Aggregation

### F.2 Support-Anfragen

**Input:** Kunden-Anfragen

**Aktivitäten:**
1. Anfrage im Logbook dokumentieren
2. Lösung finden
3. Ggf. Fix deployen
4. Kommunikation mit Kunde

**Output:**
- Gelöste Tickets
- Dokumentation im Logbook

```
📝 FitFlow Beispiel - Support:
────────────────────────────────────
2025-02-05: Lisa meldet "Buchung verschwindet"
→ Bug gefunden: Race Condition bei gleichzeitiger Buchung
→ Fix deployed (commit def456)
→ Lisa informiert
```

**Automatisierungspotenzial:**
- [ ] Ticket-System Integration
- [ ] Automatische Bug-Kategorisierung

### F.3 Feature-Requests & Erweiterungen

**Input:** Kunden-Wünsche, eigene Ideen

**Aktivitäten:**
1. Request dokumentieren
2. Priorisieren (ggf. Multi-AI Feedback)
3. Neuen Mini-Lifecycle starten (B-E)

**Output:**
- Backlog gepflegt
- Ggf. Angebot für Erweiterung

```
📝 FitFlow Beispiel - Feature-Request:
────────────────────────────────────
Lisa: "Können wir Wartelisten hinzufügen?"
→ Im Backlog, geschätzt 4h
→ Angebot: 400€
```

**Automatisierungspotenzial:**
- [ ] Feature-Request-Formular
- [ ] Automatische Aufwandsschätzung

---

## Phase G: Incident Response

**Ziel:** Schnelle Reaktion bei Problemen

### G.1 Incident Detection

**Input:** Alert, Kunden-Meldung, eigene Beobachtung

**Aktivitäten:**
1. Problem verifizieren
2. Severity einschätzen
3. Logbook-Eintrag (Incident Start)

**Output:**
- Dokumentierter Incident

```
📝 FitFlow Beispiel - Incident:
────────────────────────────────────
2025-03-01 09:15: App nicht erreichbar
Severity: HIGH (Produktiv-Ausfall)
Erste Analyse: Supabase Region-Outage
```

**Automatisierungspotenzial:**
- [ ] Automatische Incident-Erkennung
- [ ] Severity-Klassifizierung
- [ ] Stakeholder-Notification

### G.2 Investigation & Fix

**Input:** Incident-Details

**Aktivitäten:**
1. Root-Cause-Analyse
2. Fix entwickeln (oder warten bei externem Problem)
3. Fix deployen
4. Verifizieren

**Output:**
- Problem gelöst
- Root-Cause dokumentiert

**Automatisierungspotenzial:**
- [ ] Log-Analyse
- [ ] Runbook-Lookup
- [ ] Automated Rollback

### G.3 Post-Mortem

**Input:** Gelöster Incident

**Aktivitäten:**
1. Timeline dokumentieren
2. Root-Cause festhalten
3. Preventive Actions definieren
4. Kunden informieren (wenn relevant)

**Output:**
- G1-PostMortem.md (Artefakt)
- Lessons Learned

```
📝 FitFlow Beispiel - Post-Mortem:
────────────────────────────────────
Incident: 2025-03-01 Outage
Duration: 45 Min
Root Cause: Supabase Frankfurt Region Outage
Our Response: Monitoring alert nach 5 Min, Status-Page gecheckt
Action: Uptime-Monitoring auf 1-Min-Intervall
```

**Automatisierungspotenzial:**
- [ ] Post-Mortem-Template
- [ ] Automatische Timeline aus Logs

---

## Phase H: Decommission

**Ziel:** Sauberes Projektende

### H.1 Entscheidung zum Ende

**Input:** Kunden-Wunsch, Vertragslaufzeit, andere Gründe

**Aktivitäten:**
1. Kündigung/Ende dokumentieren
2. Daten-Handling klären mit Kunde

**Output:**
- Logbook-Eintrag
- Klarer Zeitplan

```
📝 FitFlow Beispiel (hypothetisch):
────────────────────────────────────
2026-01-01: Lisa verkauft Studios, braucht App nicht mehr
Daten: Export als CSV gewünscht
Deadline: 2026-01-31
```

### H.2 Daten-Export & Übergabe

**Input:** Kunden-Anforderungen

**Aktivitäten:**
1. Daten exportieren
2. An Kunde übergeben
3. Dokumentation übergeben (falls gewünscht)

**Output:**
- Daten-Export (Artefakt)
- Übergabe dokumentiert

**Automatisierungspotenzial:**
- [ ] Automatischer Daten-Export
- [ ] Übergabe-Checkliste

### H.3 Shutdown

**Input:** Übergabe abgeschlossen

**Aktivitäten:**
1. App offline nehmen
2. Datenbank löschen (nach Frist)
3. Domain freigeben
4. Coolify-Projekt archivieren

**Output:**
- Ressourcen freigegeben
- H1-Decommission.md (Artefakt)

**Automatisierungspotenzial:**
- [ ] Scheduled Shutdown
- [ ] Automatische Ressourcen-Cleanup

### H.4 Projekt archivieren

**Input:** Alles abgeschlossen

**Aktivitäten:**
1. Finales Logbook-Entry
2. Projekt-Status: "archived"
3. Lessons Learned dokumentieren

**Output:**
- Archiviertes Projekt im Hub
- Wissen erhalten

```
📝 FitFlow Beispiel - Archivierung:
────────────────────────────────────
Projekt: FitFlow Booking
Laufzeit: 2025-01-06 bis 2026-01-31 (13 Monate)
Umsatz: 2.800€ + 400€ (Warteliste) = 3.200€
Learnings: Kursbuchungen gut mit Lovable machbar
```

---

## Zusammenfassung: Automatisierungspotenzial pro Phase

| Phase | Top-Kandidaten für Automatisierung |
|-------|-----------------------------------|
| **A** | Multi-AI Feedback, Discovery→Projektbeschreibung |
| **B** | Schema-Generierung, Prompt-Erstellung, Auto-Commits |
| **C** | Multi-Tool Reviews, Finding-Aggregation, Fix-Prompts |
| **D** | DNS-Automation, Env-Sync |
| **E** | Smoke-Tests, Deploy-Notifications |
| **F** | Monitoring, Alerts, Health-Reports |
| **G** | Incident-Detection, Runbooks |
| **H** | Daten-Export, Ressourcen-Cleanup |

---

## Quick-Wins (sofort umsetzbar)

1. **Multi-AI Feedback API** - Alle 4 AIs haben APIs, parallelisierbar
2. **Templates** - Projektbeschreibung, A1-Acceptance, Review-Prompts
3. **Logbook-Typen standardisieren** - discovery, decision, milestone, incident, etc.
4. **Hub als Single Source of Truth** - Alles läuft durch den Hub

---

## Nächste Schritte zur Automatisierung

1. [ ] Multi-AI Feedback-Tool bauen (CLI oder Web)
2. [ ] Template-Bibliothek anlegen
3. [ ] Hub-Integration für automatische Logbook-Einträge
4. [ ] Coolify/Supabase API-Wrapper

---

*Dieser Prozess wird iterativ verbessert basierend auf weiteren Projekten.*
