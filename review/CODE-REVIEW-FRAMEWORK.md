# Code Review Framework - AppWerkstatt

**Version:** 1.0
**Erstellt:** 2025-12-31
**Zweck:** Systematisches, vollständiges Code Review für Web-Apps

---

## Übersicht

Dieses Framework definiert **8 Review-Bereiche** mit insgesamt **~80 Prüfpunkten**. Jeder Bereich kann unabhängig geprüft werden.

| # | Bereich | Fokus | Kritikalität |
|---|---------|-------|--------------|
| 1 | Security | Authentifizierung, Injection, Secrets | 🔴 Hoch |
| 2 | Data Protection | RLS, Datenzugriff, Privacy | 🔴 Hoch |
| 3 | Error Handling | Fehlerbehandlung, User Feedback | 🟠 Mittel |
| 4 | TypeScript & Code Quality | Types, Linting, Best Practices | 🟠 Mittel |
| 5 | React & Components | Patterns, Performance, Hooks | 🟠 Mittel |
| 6 | Accessibility | a11y, Keyboard, Screen Reader | 🟡 Mittel |
| 7 | UX Completeness | States, Feedback, Edge Cases | 🟡 Mittel |
| 8 | Configuration & Deployment | Env Vars, Build, SEO | 🟡 Niedrig |

---

## Bewertungssystem

Für jeden Prüfpunkt:

| Symbol | Bedeutung | Aktion |
|--------|-----------|--------|
| ✅ | Bestanden | Keine Aktion nötig |
| ⚠️ | Warnung | Sollte behoben werden |
| ❌ | Fehler | Muss vor Release gefixt werden |
| ⏭️ | Nicht anwendbar | Überspringen |
| 🔍 | Unklar | Manuelle Prüfung nötig |

---

## 1. Security 🔴

### 1.1 Authentifizierung & Autorisierung

| ID | Prüfpunkt | Wie prüfen | Status |
|----|-----------|------------|--------|
| SEC-AUTH-001 | Auth-Provider korrekt konfiguriert | Supabase Dashboard prüfen | |
| SEC-AUTH-002 | Session-Handling sicher (httpOnly, secure) | Network Tab / Cookies prüfen | |
| SEC-AUTH-003 | Logout löscht alle Tokens/State | Logout testen, Storage prüfen | |
| SEC-AUTH-004 | Protected Routes haben Auth-Check | `ProtectedRoute` / Auth Guards prüfen | |
| SEC-AUTH-005 | API-Calls haben Auth-Header | Network Tab bei API-Calls | |

### 1.2 Input Validation & Injection

| ID | Prüfpunkt | Wie prüfen | Status |
|----|-----------|------------|--------|
| SEC-INJ-001 | Kein `dangerouslySetInnerHTML` ohne Sanitizing | Grep: `dangerouslySetInnerHTML` | |
| SEC-INJ-002 | SQL-Injection verhindert (Parameterized Queries) | Supabase Client Usage prüfen | |
| SEC-INJ-003 | XSS verhindert (User-Input escaped) | User-Input in JSX prüfen | |
| SEC-INJ-004 | URL-Parameter validiert | `useParams`, `useSearchParams` prüfen | |
| SEC-INJ-005 | File Upload validiert (Typ, Größe) | Upload-Handler prüfen | |
| SEC-INJ-006 | `javascript:` URLs blockiert | Links mit User-Input prüfen | |

### 1.3 Secrets & Credentials

| ID | Prüfpunkt | Wie prüfen | Status |
|----|-----------|------------|--------|
| SEC-SEC-001 | Keine Secrets im Code/Repo | Grep: API_KEY, SECRET, PASSWORD, TOKEN | |
| SEC-SEC-002 | `.env` in `.gitignore` | `.gitignore` prüfen | |
| SEC-SEC-003 | VITE_* Variablen sind public-safe | Nur ANON keys, keine Service Keys | |
| SEC-SEC-004 | OAuth Tokens sicher gespeichert | DB-Verschlüsselung oder Supabase Vault | |
| SEC-SEC-005 | Externe API-Keys nur in Edge Functions | Keine API-Keys im Frontend | |

### 1.4 External Resources

| ID | Prüfpunkt | Wie prüfen | Status |
|----|-----------|------------|--------|
| SEC-EXT-001 | `window.open` hat `noopener,noreferrer` | Grep: `window.open` | |
| SEC-EXT-002 | External Links haben `rel="noopener"` | `<a>` Tags mit `target="_blank"` | |
| SEC-EXT-003 | CORS korrekt konfiguriert | Supabase / Edge Function Config | |
| SEC-EXT-004 | CSP-Header gesetzt (wenn möglich) | Response Headers prüfen | |

---

## 2. Data Protection 🔴

### 2.1 Row Level Security (RLS)

| ID | Prüfpunkt | Wie prüfen | Status |
|----|-----------|------------|--------|
| RLS-001 | RLS auf allen User-Tabellen aktiviert | Supabase Dashboard | |
| RLS-002 | SELECT-Policies: User sieht nur eigene Daten | Policy-Definition prüfen | |
| RLS-003 | INSERT-Policies: User kann nur für sich erstellen | Policy mit `auth.uid()` | |
| RLS-004 | UPDATE-Policies: User kann nur eigene Daten ändern | `owner_id = auth.uid()` | |
| RLS-005 | DELETE-Policies: User kann nur eigene Daten löschen | Policy prüfen oder bewusst fehlen | |
| RLS-006 | Keine `USING (true)` Policies (außer public Daten) | Policies auflisten | |

### 2.2 Data Access

| ID | Prüfpunkt | Wie prüfen | Status |
|----|-----------|------------|--------|
| DATA-001 | Sensible Daten nicht in localStorage | `localStorage` Usage prüfen | |
| DATA-002 | Keine PII in URLs/Query Params | Routing prüfen | |
| DATA-003 | Keine sensiblen Daten in Console Logs | Grep: `console.log` mit Variablen | |
| DATA-004 | Error Messages leaken keine Interna | Error Handling prüfen | |

---

## 3. Error Handling 🟠

### 3.1 Frontend Error Handling

| ID | Prüfpunkt | Wie prüfen | Status |
|----|-----------|------------|--------|
| ERR-FE-001 | ErrorBoundary vorhanden | `ErrorBoundary` Komponente suchen | |
| ERR-FE-002 | try-catch um kritische Operationen | Clipboard, Storage, etc. | |
| ERR-FE-003 | Async Errors werden gefangen | `.catch()` oder try-catch in async | |
| ERR-FE-004 | Fehler werden nicht geschluckt | Kein leeres `catch {}` | |
| ERR-FE-005 | User bekommt Feedback bei Fehlern | Toast/Alert bei Errors | |

### 3.2 API Error Handling

| ID | Prüfpunkt | Wie prüfen | Status |
|----|-----------|------------|--------|
| ERR-API-001 | API-Errors haben User-freundliche Messages | Error Responses prüfen | |
| ERR-API-002 | HTTP Status Codes korrekt (400, 401, 403, 500) | Edge Functions prüfen | |
| ERR-API-003 | Retry-Logic für transiente Fehler | React Query retry config | |
| ERR-API-004 | Timeout-Handling | Fetch/Axios Timeout Config | |

### 3.3 Form Validation

| ID | Prüfpunkt | Wie prüfen | Status |
|----|-----------|------------|--------|
| ERR-FORM-001 | Client-Side Validation vorhanden | Form-Komponenten prüfen | |
| ERR-FORM-002 | Validation Messages sind hilfreich | Error-Messages prüfen | |
| ERR-FORM-003 | Server-Side Validation als Backup | Edge Functions / RLS | |

---

## 4. TypeScript & Code Quality 🟠

### 4.1 TypeScript Configuration

| ID | Prüfpunkt | Wie prüfen | Status |
|----|-----------|------------|--------|
| TS-CFG-001 | `strict: true` aktiviert | `tsconfig.json` | |
| TS-CFG-002 | `noImplicitAny: true` | `tsconfig.json` | |
| TS-CFG-003 | `strictNullChecks: true` | `tsconfig.json` | |
| TS-CFG-004 | Keine TypeScript Errors | `tsc --noEmit` | |

### 4.2 Type Safety

| ID | Prüfpunkt | Wie prüfen | Status |
|----|-----------|------------|--------|
| TS-TYPE-001 | Keine `any` Types (außer begründet) | Grep: `: any` | |
| TS-TYPE-002 | API Responses haben Types | Supabase Types generiert | |
| TS-TYPE-003 | Props haben Interface/Type | Komponenten prüfen | |
| TS-TYPE-004 | Keine `@ts-ignore` (außer begründet) | Grep: `@ts-ignore` | |

### 4.3 Code Quality

| ID | Prüfpunkt | Wie prüfen | Status |
|----|-----------|------------|--------|
| QUAL-001 | Keine unbenutzten Variablen/Imports | ESLint / IDE Warnings | |
| QUAL-002 | Keine TODO/FIXME für Release-Blocker | Grep: `TODO`, `FIXME` | |
| QUAL-003 | Konsistente Formatierung | Prettier | |
| QUAL-004 | Keine Magic Numbers/Strings | Constants extrahiert | |
| QUAL-005 | Funktionen < 50 Zeilen (Richtwert) | Große Funktionen prüfen | |
| QUAL-006 | Dateien < 300 Zeilen (Richtwert) | Große Dateien prüfen | |

---

## 5. React & Components 🟠

### 5.1 Component Patterns

| ID | Prüfpunkt | Wie prüfen | Status |
|----|-----------|------------|--------|
| REACT-001 | UI-Komponenten haben `forwardRef` | Button, Input, etc. | |
| REACT-002 | Keine Props Drilling (>3 Ebenen) | Component Tree prüfen | |
| REACT-003 | Context für globalen State | Auth, Theme, etc. | |
| REACT-004 | Komponenten sind fokussiert (SRP) | Große Komponenten prüfen | |

### 5.2 Hooks

| ID | Prüfpunkt | Wie prüfen | Status |
|----|-----------|------------|--------|
| HOOK-001 | useEffect Dependencies vollständig | ESLint exhaustive-deps | |
| HOOK-002 | Cleanup in useEffect (wenn nötig) | Subscriptions, Timers | |
| HOOK-003 | useMemo/useCallback sinnvoll eingesetzt | Nicht überall, nur bei Performance | |
| HOOK-004 | Custom Hooks extrahiert für Wiederverwendung | Komplexe Logik in Hooks | |

### 5.3 Performance

| ID | Prüfpunkt | Wie prüfen | Status |
|----|-----------|------------|--------|
| PERF-001 | Keine unnötigen Re-Renders | React DevTools Profiler | |
| PERF-002 | Listen haben `key` Props | `.map()` Aufrufe prüfen | |
| PERF-003 | Lazy Loading für große Komponenten | `React.lazy` / Suspense | |
| PERF-004 | Images optimiert (WebP, lazy loading) | `<img>` Tags prüfen | |

---

## 6. Accessibility 🟡

### 6.1 Keyboard Navigation

| ID | Prüfpunkt | Wie prüfen | Status |
|----|-----------|------------|--------|
| A11Y-KEY-001 | Alle interaktiven Elemente per Tab erreichbar | Tab durch App navigieren | |
| A11Y-KEY-002 | Focus-Indicator sichtbar | Tab-Navigation visuell prüfen | |
| A11Y-KEY-003 | Escape schließt Modals/Dialogs | Keyboard testen | |
| A11Y-KEY-004 | Custom Controls haben Keyboard Handler | Buttons, Checkboxes, etc. | |

### 6.2 Screen Reader

| ID | Prüfpunkt | Wie prüfen | Status |
|----|-----------|------------|--------|
| A11Y-SR-001 | Buttons haben Label (Text oder aria-label) | Icon-only Buttons prüfen | |
| A11Y-SR-002 | Forms haben Labels | `<label>` oder `aria-labelledby` | |
| A11Y-SR-003 | Images haben alt-Text | `<img>` Tags prüfen | |
| A11Y-SR-004 | Landmarks vorhanden (header, main, nav) | Semantic HTML prüfen | |

### 6.3 Visual

| ID | Prüfpunkt | Wie prüfen | Status |
|----|-----------|------------|--------|
| A11Y-VIS-001 | Farb-Kontrast ausreichend (4.5:1) | Contrast Checker Tool | |
| A11Y-VIS-002 | Nicht nur Farbe für Information | Error States, Status | |
| A11Y-VIS-003 | Text skalierbar (keine px für font-size) | rem/em prüfen | |

---

## 7. UX Completeness 🟡

### 7.1 Loading States

| ID | Prüfpunkt | Wie prüfen | Status |
|----|-----------|------------|--------|
| UX-LOAD-001 | Initial Loading Indicator | Skeleton oder Spinner | |
| UX-LOAD-002 | Button Loading State | Spinner + disabled während Submit | |
| UX-LOAD-003 | Kein Layout Shift beim Laden | Content Placeholder | |

### 7.2 Empty States

| ID | Prüfpunkt | Wie prüfen | Status |
|----|-----------|------------|--------|
| UX-EMPTY-001 | Leere Listen haben Platzhalter | "Keine Einträge" Message | |
| UX-EMPTY-002 | CTA in Empty States | "Erstelle dein erstes..." | |
| UX-EMPTY-003 | Keine leeren weißen Flächen | Alle Szenarien durchspielen | |

### 7.3 Error States

| ID | Prüfpunkt | Wie prüfen | Status |
|----|-----------|------------|--------|
| UX-ERR-001 | API-Fehler werden angezeigt | Toast oder Inline Error | |
| UX-ERR-002 | Retry-Option bei Fehlern | "Erneut versuchen" Button | |
| UX-ERR-003 | Form-Errors inline angezeigt | Bei invaliden Feldern | |

### 7.4 User Feedback

| ID | Prüfpunkt | Wie prüfen | Status |
|----|-----------|------------|--------|
| UX-FB-001 | Success Toast nach Aktionen | Speichern, Löschen, etc. | |
| UX-FB-002 | Bestätigung vor destruktiven Aktionen | Löschen-Dialoge | |
| UX-FB-003 | Undo-Option (wenn sinnvoll) | Nach Löschen | |

---

## 8. Configuration & Deployment 🟡

### 8.1 Environment Variables

| ID | Prüfpunkt | Wie prüfen | Status |
|----|-----------|------------|--------|
| CFG-ENV-001 | `.env.example` vorhanden | Datei prüfen | |
| CFG-ENV-002 | Alle Env Vars dokumentiert | README oder .env.example | |
| CFG-ENV-003 | Production Env Vars gesetzt | Deployment Platform prüfen | |

### 8.2 Build & Bundle

| ID | Prüfpunkt | Wie prüfen | Status |
|----|-----------|------------|--------|
| CFG-BUILD-001 | Build läuft ohne Errors | `npm run build` | |
| CFG-BUILD-002 | Keine Console Warnings im Build | Build Output prüfen | |
| CFG-BUILD-003 | Bundle Size akzeptabel | `vite-bundle-visualizer` | |

### 8.3 SEO & Meta

| ID | Prüfpunkt | Wie prüfen | Status |
|----|-----------|------------|--------|
| CFG-SEO-001 | `<title>` korrekt | `index.html` | |
| CFG-SEO-002 | `<meta description>` vorhanden | `index.html` | |
| CFG-SEO-003 | Open Graph Tags (wenn public) | `og:title`, `og:image` | |
| CFG-SEO-004 | Favicon vorhanden | Browser Tab prüfen | |

### 8.4 Deployment

| ID | Prüfpunkt | Wie prüfen | Status |
|----|-----------|------------|--------|
| CFG-DEP-001 | HTTPS erzwungen | URL prüfen | |
| CFG-DEP-002 | Redirects konfiguriert (SPA) | 404 → index.html | |
| CFG-DEP-003 | Error Tracking Setup (Sentry o.ä.) | Code prüfen | |

---

## Review-Durchführung

### Vorbereitung

1. **Code auschecken** (aktueller Stand)
2. **Dependencies installieren** (`npm install`)
3. **Build testen** (`npm run build`)
4. **App lokal starten** (`npm run dev`)

### Reihenfolge

1. **Security (1) + Data Protection (2)** zuerst - Blocker identifizieren
2. **Error Handling (3)** - Stabilität sichern
3. **TypeScript (4) + React (5)** - Code Quality
4. **A11y (6) + UX (7)** - User Experience
5. **Config (8)** - Deployment Readiness

### Output-Format

```markdown
# Code Review Report

**Projekt:** [Name]
**Reviewer:** [Tool/Person]
**Datum:** YYYY-MM-DD
**Commit:** [Hash]

## Zusammenfassung

| Bereich | ✅ | ⚠️ | ❌ | Gesamt |
|---------|----|----|----|----|
| 1. Security | X | X | X | X |
| 2. Data Protection | X | X | X | X |
| ... | | | | |

## Kritische Findings (❌)

| ID | Problem | Datei | Zeile | Fix |
|----|---------|-------|-------|-----|
| SEC-INJ-001 | XSS in User-Display | UserCard.tsx | 42 | Escape oder sanitize |

## Warnungen (⚠️)

[...]

## Empfehlungen

1. [Priorität 1]
2. [Priorität 2]
3. [...]
```

---

## Für AI-Reviewer (Cursor, Antigravity, Lovable)

### Prompt-Template

```
Du führst ein Code Review durch nach dem AppWerkstatt Code Review Framework.

Lies zuerst: docs/review/CODE-REVIEW-FRAMEWORK.md

Dann prüfe systematisch jeden Bereich (1-8) und dokumentiere:
- ✅ Bestanden
- ⚠️ Warnung (mit Begründung)
- ❌ Fehler (mit Datei, Zeile, Fix-Vorschlag)

Fokus-Reihenfolge:
1. Security + Data Protection (Blocker finden)
2. Error Handling
3. Rest

Gib am Ende eine priorisierte Liste der Top 10 Findings.
```

### Tipps für AI-Reviewer

- **Grep nutzen** für Pattern-Suche (`any`, `TODO`, `console.log`)
- **Nicht alles ist ein Problem** - Kontext beachten
- **False Positives markieren** wenn unklar
- **Fixes vorschlagen**, nicht nur Probleme auflisten

---

## Changelog

| Version | Datum | Änderung |
|---------|-------|----------|
| 1.0 | 2025-12-31 | Initial Release |

---

*Dieses Framework ist ein Living Document. Feedback und Ergänzungen willkommen.*
