# Cursor Code Review Schema

**Version:** 1.0
**Erstellt:** 2025-12-29
**Basiert auf:** Feedback-Synthese Runde 4 (Claude, Gemini, ChatGPT, Perplexity)

---

## Zweck

Dieses Schema ermöglicht eine systematische Code-Review von **Lovable-generiertem Code** durch **Cursor**.

**Anwendbar für:**
- React + Vite + TypeScript Projekte
- Supabase Backend (Auth + Database + RLS)
- shadcn/ui Komponenten
- Alle AI-generierten Codebases

---

## Setup vor dem Review

### Voraussetzungen

```markdown
- [ ] Cursor ist mit dem Repo verbunden
- [ ] Alle Dependencies installiert (npm install erfolgreich)
- [ ] .env.local existiert mit korrekten Supabase Keys
- [ ] App startet lokal (npm run dev)
```

### Cursor Einstellungen

- Model: Claude 3.5 Sonnet oder besser
- Context: `@Codebase` aktiviert
- Mode: Composer oder Chat

---

## Severity Levels

```
🔴 P0 - BLOCKER
   Security-Lücke, Crash, Datenverlust
   → Muss vor Merge gefixt werden

🟠 P1 - SHOULD FIX
   Feature fehlt/kaputt, schlechte UX, Error ohne Feedback
   → Sollte vor Go-Live gefixt werden

🟡 P2 - NICE-TO-HAVE
   Code-Qualität, Performance, Refactoring
   → Backlog für später
```

---

## Die 5 Review-Prompts

Führe diese Prompts **nacheinander** in Cursor aus. Jeder Prompt fokussiert einen Bereich.

---

### Prompt 1: Inventory & Architecture

```markdown
# Cursor: Codebase Inventory

Analysiere die Codebase und erstelle eine Übersicht.

## Aufgaben

1. **Dateistruktur**
   Liste alle wichtigen Ordner und ihre Funktion:
   - src/pages/ - Was sind die Routes?
   - src/components/ - Welche UI-Komponenten?
   - src/hooks/ - Welche Custom Hooks?
   - src/lib/ - Welche Utilities?

2. **Supabase Integration**
   - Wo ist der Supabase Client initialisiert?
   - Werden Environment Variables verwendet?
   - Gibt es TypeScript Types für die DB-Tabellen?

3. **Auth Flow**
   - Wo ist der Auth Context/Provider?
   - Wie werden Protected Routes implementiert?
   - Wo wird Session geprüft?

4. **Feature-Mapping**
   Erstelle eine Tabelle: Feature → Hauptkomponenten → Dateien

## Output
Markdown-Übersicht mit Dateilisten und Feature-Map.
```

---

### Prompt 2: Security Audit (P0)

```markdown
# Cursor: Security Audit

Du bist ein Security Engineer. Prüfe die Codebase auf Sicherheitsprobleme.

## 1. Row Level Security (RLS)

Prüfe für JEDE Supabase-Query:
- [ ] SELECT: Ist `owner_id = auth.uid()` gewährleistet (durch RLS oder Query)?
- [ ] INSERT: Wird `owner_id` korrekt gesetzt?
- [ ] UPDATE/DELETE: Nur auf eigene Daten?

Suche nach Anti-Patterns:
```typescript
// SCHLECHT: Kein owner_id Check
await supabase.from('projects').select('*')

// GUT: RLS übernimmt das (prüfen ob aktiviert)
// oder explizit: .eq('owner_id', user.id)
```

## 2. Input Validation

Für jedes Formular prüfen:
- [ ] Pflichtfelder werden vor Submit validiert
- [ ] Max-Length für Textfelder
- [ ] URLs werden validiert (keine javascript: URLs)

## 3. XSS Prevention

Suche nach:
```typescript
// GEFÄHRLICH
dangerouslySetInnerHTML
innerHTML =

// PRÜFEN: Markdown-Rendering sicher?
<ReactMarkdown>{userContent}</ReactMarkdown>
```

## 4. Secrets Management

- [ ] Keine API Keys hardcoded (nur VITE_* env vars)
- [ ] .env.local in .gitignore
- [ ] Kein Service Role Key im Client

## 5. Auth Flow

- [ ] Logout invalidiert Session
- [ ] Token Refresh funktioniert
- [ ] Keine Auth Tokens in localStorage (Supabase handled das)

## Output
Liste aller Security Issues mit:
- Severity (🔴 P0)
- Datei + Zeile
- Problem + Fix
```

---

### Prompt 3: Feature Verification

```markdown
# Cursor: Feature Verification

Prüfe ob alle Features korrekt implementiert sind.

## Feature 1: Auth + Layout

### Anforderungen
- Login mit Email/Password
- Session bleibt nach Refresh
- Nicht eingeloggt → /login redirect
- Sidebar Navigation
- Dark Theme

### Code-Checks
- [ ] `supabase.auth.signInWithPassword()` verwendet
- [ ] `supabase.auth.onAuthStateChange()` listener existiert
- [ ] ProtectedRoute oder ähnlicher Guard
- [ ] Session aus `supabase.auth.getSession()` initial geladen

### Edge Cases
- [ ] Ungültige Credentials → Fehlermeldung
- [ ] Refresh → Session bleibt
- [ ] Direkter URL-Zugriff ohne Login → Redirect

---

## Feature 2: Dashboard + Projekte

### Anforderungen
- Projekt-Grid (responsive)
- Karten: Name, Status-Badge, Phase, letzter Eintrag
- "Neues Projekt" Modal
- Loading/Empty/Error States

### Code-Checks
- [ ] Query: `supabase.from('projects').select()`
- [ ] Sortierung: `order('updated_at', { ascending: false })`
- [ ] Insert mit `owner_id`
- [ ] Nach Insert: Liste refresh oder optimistic update

### Edge Cases
- [ ] 0 Projekte → Empty State
- [ ] Netzwerkfehler → Error State
- [ ] Leerer Name → Validation

---

## Feature 3: Quick-Capture (Strg+K)

### Anforderungen
- Keyboard Shortcut Strg+K / Cmd+K
- Modal: Projekt-Dropdown, Typ, Titel
- Enter = Speichern, Escape = Abbrechen
- Toast nach Speichern

### Code-Checks
- [ ] `useEffect` mit `keydown` listener
- [ ] `event.metaKey || event.ctrlKey` geprüft
- [ ] `event.preventDefault()` um Browser-Suche zu blocken
- [ ] **WICHTIG:** Cleanup im useEffect return!

### Edge Cases
- [ ] Shortcut funktioniert auf allen Seiten
- [ ] Shortcut NICHT auf Login-Seite
- [ ] Keine Projekte → Modal disabled oder Hinweis?
- [ ] Leerer Titel → Validation

---

## Feature 4: Projekt-Detail Header

### Anforderungen
- Route /projects/:id
- Back-Link zu Dashboard
- Status/Phase Dropdowns (live speichern)
- Tool-Links mit Icons
- "+ Link" Modal
- Tab-Navigation

### Code-Checks
- [ ] `useParams()` für project_id
- [ ] Query: `supabase.from('projects').select().eq('id', id).single()`
- [ ] Status/Phase Update: `.update({ status }).eq('id', id)`
- [ ] Links Query: `supabase.from('project_links').select().eq('project_id', id)`

### Edge Cases
- [ ] ID existiert nicht → 404 oder Error State
- [ ] Back-Button funktioniert

---

## Feature 5: Logbook Liste + Pinning

### Anforderungen
- Chronologische Liste (neueste oben)
- Pin-Icon toggle
- Type-Badge mit Farbe
- Content-Preview (gekürzt)
- Filter: Alle/Gepinnte/Nach Typ
- Expand inline

### Code-Checks
- [ ] Query: `order('created_at', { ascending: false })`
- [ ] Pin Toggle: `.update({ is_pinned: !current }).eq('id', entryId)`
- [ ] Optimistic UI Update

### Edge Cases
- [ ] 0 Einträge → Empty State
- [ ] Sehr langer Content → Preview funktioniert

---

## Feature 6: Neuer Eintrag Modal

### Anforderungen
- Titel (Pflicht), Typ-Dropdown, Content-Textarea, Pin-Checkbox
- Cmd+Enter = Speichern
- Escape = Abbrechen
- Toast + Liste refresh

### Code-Checks
- [ ] Titel-Validation vor Insert
- [ ] Insert mit `project_id`, `owner_id`, `entry_type`, `is_pinned`
- [ ] Keyboard Handler für Cmd+Enter

### Edge Cases
- [ ] Leerer Titel → Validation
- [ ] Netzwerkfehler → Error Toast

---

## Feature 7: AI-Export

### Anforderungen
- Button im Header
- Quick (nur gepinnte) / Full (gepinnte + letzte 20)
- Markdown generieren
- In Zwischenablage kopieren
- Toast

### Code-Checks
- [ ] Quick Query: `.eq('is_pinned', true)`
- [ ] Full Query: `.limit(20)` + merge mit pinned
- [ ] `navigator.clipboard.writeText()`
- [ ] Try/Catch für Clipboard API

### Edge Cases
- [ ] Keine gepinnten → Hinweis oder leerer Abschnitt
- [ ] Clipboard API nicht verfügbar → Fallback/Error

---

## Output
Tabelle pro Feature:

| Feature | Status | Issues | Notes |
|---------|--------|--------|-------|
| 1. Auth | ✅/⚠️/❌ | ... | ... |
```

---

### Prompt 4: Code Quality (P1)

```markdown
# Cursor: Code Quality Audit

Prüfe TypeScript, React Patterns und Error Handling.

## 1. TypeScript

### Suche nach `any`
```typescript
// SCHLECHT
const data: any = ...
function handle(e: any) { ... }
(response as any).data
```

### Type Definitionen
- [ ] Interfaces für DB-Tabellen vorhanden? (Project, LogbookEntry, ProjectLink)
- [ ] Props für alle Komponenten typisiert?
- [ ] Event Handler typisiert? (React.MouseEvent, etc.)

### Enums/Union Types
```typescript
// GUT
type EntryType = 'note' | 'decision' | 'milestone' | ...
type ProjectStatus = 'active' | 'on_hold' | 'archived'

// SCHLECHT
status: string
```

---

## 2. React Patterns

### useEffect Probleme
```typescript
// PROBLEM: Fehlende Dependency
useEffect(() => {
  fetchData(userId)
}, []) // userId fehlt!

// PROBLEM: Fehlender Cleanup
useEffect(() => {
  const sub = channel.subscribe()
  // Kein return () => sub.unsubscribe()
}, [])

// PROBLEM: Infinite Loop
useEffect(() => {
  setData([...data, newItem])
}, [data]) // data ändert sich → Loop
```

### Re-render Probleme
```typescript
// PROBLEM: Neue Funktion bei jedem Render
{items.map(item => (
  <Item onClick={() => handleClick(item.id)} />
))}

// PROBLEM: Inline Objekt als Prop
<Component style={{ margin: 10 }} />
```

### Keys in Listen
```typescript
// SCHLECHT: Index als Key
{items.map((item, i) => <Item key={i} />)}

// GUT: Unique ID als Key
{items.map(item => <Item key={item.id} />)}
```

---

## 3. Error Handling

### Supabase Calls
```typescript
// SCHLECHT: Kein Error Check
const { data } = await supabase.from('x').select()
setData(data) // data kann null sein!

// GUT: Error Handling
const { data, error } = await supabase.from('x').select()
if (error) {
  toast({ title: 'Fehler', variant: 'destructive' })
  return
}
setData(data ?? [])
```

### Loading States
- [ ] Jede async Operation hat isLoading State
- [ ] Buttons disabled während Loading
- [ ] Skeleton oder Spinner während Laden

### Error Boundary
- [ ] Gibt es eine Error Boundary Komponente?
- [ ] Ist sie im Root eingebunden?

---

## 4. Lovable-spezifische Probleme

### Halluzinierte Imports
Prüfe JEDEN Import - existiert die Datei/das Symbol?
```typescript
import { useToast } from '@/hooks/use-toast'  // Existiert?
import { SomeIcon } from 'lucide-react'       // Installiert?
```

### Redundanter Code
- [ ] Gibt es duplizierte Funktionen?
- [ ] Copy-Paste zwischen ähnlichen Komponenten?
- [ ] Utility-Funktionen die mehrfach definiert sind?

### Inkonsistente Patterns
- [ ] Mal async/await, mal .then()?
- [ ] Mal Toast, mal console.error?
- [ ] Mal controlled, mal uncontrolled Inputs?

### Zombie-Code
- [ ] Unbenutzte Imports
- [ ] Unbenutzte Variablen
- [ ] Auskommentierter Code
- [ ] console.log Statements

---

## Output
Liste aller Code Quality Issues mit Severity und Fix-Vorschlag.
```

---

### Prompt 5: Polish & Accessibility (P2)

```markdown
# Cursor: Polish & Accessibility

Prüfe UX-Details und Accessibility.

## 1. Accessibility

### Keyboard Navigation
- [ ] Alle interaktiven Elemente mit Tab erreichbar
- [ ] Focus Ring sichtbar
- [ ] Modals: Focus trapped (Tab bleibt im Modal)
- [ ] Escape schließt Modals

### Screen Readers
- [ ] Form Labels haben `htmlFor`
- [ ] Icon-Buttons haben `aria-label`
- [ ] Modals haben `role="dialog"` und `aria-labelledby`

### Farben
- [ ] Nicht nur Farbe für Information (Status-Badges auch Text)
- [ ] Ausreichend Kontrast

---

## 2. UX Konsistenz

### Toast/Feedback
- [ ] Erfolg-Aktionen zeigen Toast
- [ ] Fehler-Aktionen zeigen Error Toast
- [ ] Konsistente Toast-Komponente

### Loading/Error States
- [ ] Alle async Aktionen haben Loading State
- [ ] Error States sind user-friendly (nicht technisch)
- [ ] Retry-Möglichkeit bei Fehlern

### Modals
- [ ] Schließen mit X-Button
- [ ] Schließen mit Escape
- [ ] Schließen mit Klick außerhalb (optional)
- [ ] Form Reset nach Schließen

---

## 3. Performance (nur offensichtliche Probleme)

- [ ] Keine Infinite Loops
- [ ] Keine offensichtlichen Memory Leaks
- [ ] Listen haben Keys
- [ ] Keine riesen Bundle-Imports

---

## Output
Liste der P2 Issues und Verbesserungsvorschläge.
```

---

## Issue Template

Für jedes gefundene Problem dieses Format verwenden:

```markdown
### [KATEGORIE-NNN] Kurztitel

**Severity:** 🔴 P0 | 🟠 P1 | 🟡 P2
**Kategorie:** Security | Crash | Feature | Quality | A11y | UX
**Datei:** src/path/to/file.tsx:123

**Problem:**
Kurze Beschreibung was falsch ist und warum es ein Problem ist.

**Impact:**
Was passiert wenn das nicht gefixt wird?

**Fix:**
```typescript
// Konkreter Code oder Anweisung
```

**Test:**
Wie kann man verifizieren dass der Fix funktioniert?
```

---

## Report Template

Nach Abschluss aller 5 Prompts diesen Report erstellen:

```markdown
# Code Review Report: [Projektname]

**Datum:** YYYY-MM-DD
**Reviewer:** Cursor
**Commit:** [SHA oder Branch]

---

## Zusammenfassung

| Severity | Anzahl |
|----------|--------|
| 🔴 P0 - Blocker | X |
| 🟠 P1 - Should Fix | X |
| 🟡 P2 - Nice-to-have | X |

**Status:** 🟢 READY | 🟡 CONDITIONAL | 🔴 BLOCKED

---

## Feature-Vollständigkeit

| Feature | Status | Notes |
|---------|--------|-------|
| 1. Auth + Layout | ✅/⚠️/❌ | ... |
| 2. Dashboard | ✅/⚠️/❌ | ... |
| 3. Quick-Capture | ✅/⚠️/❌ | ... |
| 4. Projekt-Detail | ✅/⚠️/❌ | ... |
| 5. Logbook | ✅/⚠️/❌ | ... |
| 6. Neuer Eintrag | ✅/⚠️/❌ | ... |
| 7. AI-Export | ✅/⚠️/❌ | ... |

---

## P0 Issues (Blocker)

[Alle P0 Issues hier]

---

## P1 Issues (Should Fix)

[Alle P1 Issues hier]

---

## P2 Issues (Nice-to-have)

[Alle P2 Issues hier]

---

## Empfehlungen

### Vor Merge (P0)
1. ...

### Vor Go-Live (P1)
1. ...

### Backlog (P2)
1. ...

---

## Go/No-Go Checklist

- [ ] Alle P0 Issues gefixt
- [ ] Alle Features funktionieren
- [ ] Security Audit bestanden
- [ ] Error Handling vorhanden

**Entscheidung:** GO / NO-GO
```

---

## Workflow

```
1. Setup prüfen (Checklist oben)
         ↓
2. Prompt 1: Inventory (Codebase verstehen)
         ↓
3. Prompt 2: Security (P0 zuerst!)
         ↓
4. Prompt 3: Features (Vollständigkeit)
         ↓
5. Prompt 4: Quality (TypeScript, React, Errors)
         ↓
6. Prompt 5: Polish (A11y, UX)
         ↓
7. Report erstellen
         ↓
8. Issues fixen (P0 zuerst)
         ↓
9. Re-Review wenn nötig
         ↓
10. Go/No-Go Entscheidung
```

---

## Wiederverwendung

Dieses Schema ist ein **Blueprint** für alle Lovable-Projekte.

**Anpassung für neues Projekt:**
1. Feature-Liste in Prompt 3 anpassen
2. Spezifische DB-Tabellen in Prompt 2 anpassen
3. Tech-Stack in Prompts anpassen falls nötig

---

*Schema erstellt am 2025-12-29 basierend auf Feedback von Claude, Gemini, ChatGPT, Perplexity*
