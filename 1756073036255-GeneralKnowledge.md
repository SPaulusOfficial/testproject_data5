# 🛡️ KI Guardrails – Technisch-funktionaler Rahmen für Agenten und Module
V3
Wir entwickeln eine modulare, AI-gestützte Plattform zur Automatisierung von typischen Tätigkeiten im Rahmen von Salesforce-Projekten. Die Project Assistant Suite begleitet den gesamten Projektlebenszyklus – von PreSales über Solution Design bis zu Rollout und Hypercare – und nutzt spezialisierte AI-Agenten zur Extraktion, Strukturierung und Generierung relevanter Artefakte wie Stories, Datenmodelle, Testfälle oder Schulungsunterlagen. Ziel ist es, diese Plattform zunächst intern zur Effizienzsteigerung einzusetzen und später als produktisiertes System im Markt zu etablieren.
Dieses Dokument beschreibt den verbindlichen Rahmen ("Guardrails") für alle KI-gestützten Komponenten innerhalb der Project Assistant Suite. Es definiert, was erlaubt ist, welche Technologien und Methoden verwendet werden dürfen und welche architektonischen sowie sicherheitsrelevanten Leitplanken einzuhalten sind.
## 1. 🔧 Technologischer Rahmen
- Die Plattform basiert ausschließlich auf **Open Source-Technologien**.

- Zulässige Programmiersprachen: **Python**, **TypeScript/JavaScript**.

- Services sind **containerisiert (Docker)** und für **Kubernetes** vorbereitet.

- Agenten kommunizieren ausschließlich über **REST-APIs oder Message Bus (RabbitMQ)**.

## 2. 🧠 KI-Logik &amp; Agentendesign
- KI-Funktionalitäten sind in **modulare Agenten** gekapselt.

- KI-Logik basiert auf **Langchain (Python)** in Kombination mit **Haystack** für RAG.

- Vektorsuche erfolgt über **Qdrant**, klassische Suche über **Elasticsearch**.

- KI-Agenten dürfen **keine irreversiblen Systemänderungen** vornehmen (read/write getrennt).

- Jeder Agent muss über eine **Custom UI oder CLI steuerbar** sein.

## 3. 🔁 Workflow- und Prozesssteuerung
- Die gesamte Prozesslogik wird in einer **eigenen Workflow Engine auf Basis von FastAPI + Redis** abgebildet.

- Workflows dürfen nur auf expliziten Triggern (Event oder API) starten.

- Prozesszustände sind persistent gespeichert und versionierbar.

- Kein stilles Überspringen von Phasen oder Rückmeldungen erlaubt.

## 4. 🔐 Sicherheit &amp; Compliance
- Zugriff wird über **OAuth2/OIDC** gesteuert (z. B. Auth0, Keycloak).

- Secrets und Tokens werden ausschließlich über **Vault** verwaltet.

- DSGVO-Vorgaben sind einzuhalten (inkl. Logging, Einwilligungen, Löschbarkeit).

- **Anonymisierung** erfolgt:
via RegEx

- durch statische/dynamische Platzhalter

- optional durch internes LLM zur semantischen Erkennung personenbezogener Inhalte

## 5. 🧭 Datenhaltung &amp; Nachvollziehbarkeit
- Alle Artefakte (Stories, Datenmodelle, Releases, Reports) sind versioniert.

- Änderungen werden über die **Change Engine** mit Graph-Struktur verfolgt.

- Jeder Agent muss seine Ausgaben speichern und referenzierbar machen (Traceability).

- Keine Blackbox-Logik ohne Logging erlaubt.

## 6. 📊 Monitoring &amp; Qualitätssicherung
- Jeder Service meldet Metriken an **Prometheus** und Logs an **Loki**.

- Fehler werden über **Sentry** erfasst und getrackt.

- Agentenverhalten wird im UI visualisiert und kann durch User bewertet werden.

- Keine Agentenentscheidung darf automatisch in Produktion übernommen werden.

## 7. ❗ Einschränkungen
- Kein direkter Zugriff auf Salesforce-Produktivsysteme ohne Userfreigabe

- Kein Einsatz von Closed-Source KI-Modellen in der Standardauslieferung

- Kein Persistieren personenbezogener Rohdaten ohne Anonymisierung

Dieses Dokument ist für alle Contributor und Agent-Instanzen bindend. Es darf nur mit Review durch den Security- und AI-Verantwortlichen geändert werden.
Salesfive&nbsp;– UI&nbsp;Implementation Guidelines
Copy‑paste this spec directly into your design‑system repository or provide it to an AI coding assistant. It contains every brand‑token, typographic rule and component spec required to recreate Salesfive digital interfaces (web‑app, presentations, landing pages).
1 Grundlayout &amp; Struktur
BereichAbmessungen &amp; VerhaltenInhalt / Komponenten
Linke Side-Bar
• Feste Breite ≈ 260 px • Volle Höhe, dunkler Farbverlauf • Flex-Spalte mit gap-y-4
1. Profil-Avatar (80 px Ø) 2. Name + E-Mail (kleine, graue Schrift) 3. Primäre Navigation (Icon + Label, Hover-Highlight, Active-State) 4. Sekundäre Links (Book a Demo, Settings, Referral)

Main-Frame
• Flex-Kolonne, flex-1 • Padding xl (links/rechts 32 px, oben 24 px) • Max-Breite 1440 px, zentriert
Header, Cards, Tabs, Tabellen, Modale, Wizards
Seitenaufbau
mermaid
KopierenBearbeiten
graph LR A[Side-Bar] --- B[Main-Frame] B --&gt; C[Page Header] C --&gt; D[Content Area]
0&nbsp;Table&nbsp;of&nbsp;Contents
Design&nbsp;Tokens (Color)
Typography&nbsp;System
Layout&nbsp;&amp; Grid
Color&nbsp;Application&nbsp;Rules
Component&nbsp;Library
States&nbsp;&amp; UX Patterns
Datavis&nbsp;(rounded‑bar charts)
Accessibility
Code&nbsp;Snippets
Implementation&nbsp;Checklist
1&nbsp;Design&nbsp;Tokens
Token
HEX
Usage
clr-open-blue
#00D5DC
Primary accent, hover highlights, infographics
clr-digital-blue
#0025D1
Brand‑primary (CTAs, links, selection)
clr-deep-blue-1
#000058
Dark hover, card headers, high‑contrast chart segment
clr-deep-blue-2
#001394
Secondary accent, badges
clr-mid-blue-1
#0051D4
Secondary controls, ghost focus
clr-mid-blue-2
#007DD7
Light table row, tooltips
clr-mid-blue-3
#00A9D9
Illustration fill, secondary charts
clr-off-white
#F7F7F9
Page background, card background
clr-black
#000000
Primary text, icons
clr-white
#FFFFFF
Text on dark backgrounds, icons
Contrast Rule
Use white typography on backgrounds #000058&nbsp;–&nbsp;#0051D4 (AAA).Use black typography on backgrounds #007DD7&nbsp;–&nbsp;#00D5DC.
2&nbsp;Typography&nbsp;System
Level
Marketing Font†
System Fallback
Size (Desktop)
Weight
H1
Helvetica&nbsp;Now
Arial
48&nbsp;–&nbsp;70&nbsp;px (≈5× Body)
700
H2
Helvetica&nbsp;Now
Arial
32&nbsp;px
700
H3
Helvetica&nbsp;Now
Arial
24&nbsp;px
700
Body
Helvetica&nbsp;Now
Arial
16&nbsp;px
400
Caption
Helvetica&nbsp;Now
Arial
12&nbsp;–&nbsp;14&nbsp;px
400
†Helvetica&nbsp;Now only on licensed, public marketing touch‑points. Internal apps use Arial Regular/Bold exclusively.
@font-face {
font-family:"HelveticaNow";
src:url("/fonts/HelveticaNow-Regular.woff2") format("woff2");
font-weight:400;
font-display:swap;
}
body {font-family:"HelveticaNow","Arial","Helvetica",sans-serif;}
3&nbsp;Layout&nbsp;&amp; Grid
Shell
Sidebar: fixed 260&nbsp;px (desktop) ⇒ collapses to 64&nbsp;px icon‑rail (&lt;&nbsp;1024&nbsp;px).
Main Frame: flex‑column, flex-1, padding&nbsp;32&nbsp;px&nbsp;24&nbsp;px&nbsp;24&nbsp;px.
Max‑width: 1440&nbsp;px, centered.
Breakpoint
Columns
Gutter
≥1536&nbsp;px (2xl)
12
32&nbsp;px
≥1280&nbsp;px (xl)
12
24&nbsp;px
≥1024&nbsp;px (lg)
12
24&nbsp;px
≥768&nbsp;px (md)
8
20&nbsp;px
&lt;768&nbsp;px (sm)
4
16&nbsp;px
Sidebar Structure (top&nbsp;→&nbsp;bottom)
Avatar&nbsp;80&nbsp;px Ø
Name &amp; email (small, grey)
Primary nav links (icon + label)
Secondary links (Book&nbsp;a&nbsp;Demo, Settings, Referral)
Dashboard Skeleton (example)
graph LR
A[Sidebar] --260px--&gt; B[Main]
B --&gt; C[Page Header]
C --&gt; D[Stats Cards]
D --&gt; E[Suggestions Grid]
4&nbsp;Color&nbsp;Application&nbsp;Rules
Palette Coverage
Guideline
Black&nbsp;&amp;&nbsp;White
80&nbsp;–&nbsp;90&nbsp;% of any screen.
Blue&nbsp;Tones
10&nbsp;–&nbsp;20&nbsp;% as highlights (CTA, charts, badges).
Do&nbsp;NOT
combine multiple mid‑blues in one small area – pick one dominant accent.
5&nbsp;Component&nbsp;Library
Buttons
Variant
Normal
Hover/Active
Disabled
Primary
BG&nbsp;#0025D1, text&nbsp;#FFF
BG&nbsp;#001394
BG&nbsp;#F7F7F9, text&nbsp;#A0A0A0
Ghost
border/text&nbsp;#0025D1
BG&nbsp;rgba(0,37,209,.08)
text/border&nbsp;#C0C4CC
Card
Radius&nbsp;12&nbsp;px
Shadow&nbsp;sm (0&nbsp;1px&nbsp;2px&nbsp;rgba(0,0,0,.06)) → md on hover.
Stat Card (Dark)
BG #000058, text #FFF, icon badge 20&nbsp;px Ø Open‑Blue.
Multi‑Select Dropdown
Checkbox + label 14&nbsp;px.
Check&nbsp;icon #00D5DC.
Scrollbar thumb #0051D4.
Progress Bar
--track:#E5E7EB;
--fill:#0025D1; /* width: calc(%%) */
height:4px; border-radius:4px;
Accordion / Workspace Tree
Up to 3 hierarchy levels.
Drag‑handle icon (≡) for re‑ordering.
Inline table inside each node (columns: Module, Sub‑Module, Technology, Complexity, Effort).
Modal / Wizard
Centered max-w-2xl, radius&nbsp;20&nbsp;px.
Step indicator bar at top (fill&nbsp;#0025D1).
Footer with Back (ghost) &amp; Next (primary, disabled until valid).
6&nbsp;States&nbsp;&amp; UX Patterns
Pattern
Behaviour
Hover
components raise sm→md shadow or receive 5&nbsp;% tint.
Keyboard&nbsp;Focus
outline:2px solid #00D5DC; outline-offset:2px.
Unsaved&nbsp;Changes
sticky banner top; optimistic UI on save.
Animations
≤&nbsp;200&nbsp;ms ease-in-out; respect prefers-reduced-motion.
7&nbsp;Datavis – Rounded‑Bar&nbsp;Charts
.bar--marketing{fill:#0025D1; rx:42%}
.bar--support {fill:#001394; rx:42%}
.bar--perf {fill:#000058; rx:42%}
.bar--usab {fill:#00D5DC; rx:42%}
.text--inside{font-size:12px;fill:#FFF;font-weight:700}
.axis{stroke:#E5E7EB}
All bars share identical width; radius set via rx for pill‑shape.
8&nbsp;Accessibility&nbsp;Checklist
Contrast: meet WCAG&nbsp;AA (4.5:1).
Focus management: trap inside modals, return focus on close.
Keyboard path: every interactive element reachable with Tab.
Motion: reduce for users with prefers-reduced-motion.
ARIA: proper labels for dropdowns and wizard steps.
9&nbsp;Code&nbsp;Snippets
Tailwind&nbsp;Primary&nbsp;Button
CTA Label
Vue / React Layout Skeleton (pseudo‑code)
...
10&nbsp;Implementation&nbsp;Checklist
Export tokens to CSS custom properties or Tailwind theme.
Global&nbsp;CSS reset + font‑face.
Atom components (button, input, tag) in Storybook → snapshot tests.
Molecules (card, dropdown, progress).
Organisms (wizard, workspace tree, stat grid).
Routing: /dashboard, /proposal/:id, wizard /new.
State: React‑Query for async data, Context for auth/profile.
Testing: Playwright E2E + RTL unit tests.
CI/CD: ESLint, Prettier, Husky pre‑commit; preview deployments (Vercel).
Result: With these tokens, rules and patterns you can reproduce the GetGenerative‑style interface in the Salesfive brand universe – fully responsive, accessible, and ready for production.