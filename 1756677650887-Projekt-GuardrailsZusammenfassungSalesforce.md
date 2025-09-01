Projekt: [Projektname]
Version: 1.0 | Datum: 31.08.2025

Owner: [Rolle/Team]
Hinweis: Keine bestehende Projektdatei gefunden; basiert auf SF-Best Practices. Angleichen an eure finalen Guardrails empfohlen.
1. Zweck &amp; Geltungsbereich

- Einheitliche, sichere und wartbare Umsetzung in allen Umgebungen (Dev, SIT, UAT, Staging, Prod).

- Gilt für Konfiguration, Code, Integrationen, Daten, Deployments, Betrieb und KI-Nutzung.

1. Architektur &amp; Org-Strategie

- Git als Source of Truth; keine Direktänderungen in Prod.

- Modularisierung mit (Unlocked) Packages wo sinnvoll; klare Domänengrenzen.

- Sandboxes nach Zweck; Scratch Orgs für isolierte Entwicklung/PoCs.

1. Sicherheit &amp; Compliance

- Least Privilege: Permission Sets/Groups statt Profile für Rechtevergabe.

- FLS/CRUD erzwingen (Apex: with sharing, Security.stripInaccessible; Flows/LWC beachten).

- Secrets über Named Credentials/OAuth; keine Secrets im Code.

- Datenschutz/DSGVO: Lösch- und Aufbewahrungskonzepte, Verschlüsselung (Shield bei Bedarf).

1. Datenmodell &amp; Qualität

- Dokumentiertes, kanonisches ERD; keine Duplikat-Felder ohne Entscheidung (ADR).

- Externe IDs/Integrationsschlüssel stabil; Idempotenz bei Datenübernahmen.

- Volumen: selektive SOQL, Indizes, Archivierung; Big Objects falls nötig.

1. Automatisierung (Flows &amp; Apex)

- Flow-first; Code bei Komplexität/Performancebedarf.

- Pro Objekt max. 1 Before- und 1 After-Record-Triggered Flow (mit Subflows).

- Apex: Ein Trigger je Objekt, Handler/Service-Layer, vollständig bulkifiziert; keine SOQL/DML in Schleifen.

- Asynchronität für Callouts/Massenverarbeitung (Queueable/Batch/Platform Events).

1. Integrationen &amp; APIs

- Named Credentials, OAuth/mTLS; Logging mit Maskierung sensibler Daten.

- Muster: API-first; Events (Platform Events/CDC) für Entkopplung; ETL für Massendaten.

- Idempotenz, Retry/Backoff, Dead-Letter-Handling; Versionsverträge (OpenAPI) pflegen.

1. Performance &amp; Limits

- SOQL selektiv; Feldauswahl minimal; Nutzung von Indizes.

- DML gesammelt; große Workloads asynchron; CPU/Heap im Blick.

- UI: LWC Lazy Loading; @AuraEnabled(cacheable=true) für Reads.

1. Entwicklungsstandards

- Namenskonventionen konsistent (englisch, stabil); keine Hardcodings (Custom Metadata/Settings).

- Layered Architecture; keine Geschäftslogik im Trigger/Controller.

- Static Analysis (PMD/ESLint/sfdx-scanner) + Code Reviews verpflichtend.

1. Tests &amp; Qualitätssicherung

- Unit Tests: ≥75% org-weit (Ziel 85–90% Kern), pos./neg. und Bulk-Cases.

- Testdaten über Fabriken, keine Abhängigkeit von Org-Daten.

- Automatisierte Integrations-/UI-Tests für kritische Pfade in SIT/UAT.

1. CI/CD &amp; Releases

- Branching: Trunk oder leichtes Gitflow (feature/release/hotfix).

- Validate-Deploys, unveränderliche Builds/Artefakte; Feature Flags für inkrementelles Rollout.

- Rollforward bevorzugt; dokumentierter Rollback- und Datenmigrationsplan.

1. Observability &amp; Betrieb

- Strukturierte Logs, Korrelation-IDs; Limits- und Health-Monitoring.

- Incident-Prozess, SLAs/OLAs, Runbooks, blameless Postmortems.

1. Governance &amp; Entscheidungen

- CAB für produktionsrelevante Changes; RFC-Template nutzen.

- Architecture Decision Records (ADR) für wichtige Weichenstellungen.

- RACI klar (Product, Arch, Dev, QA, DevOps, Sec, Ops).

1. Dokumentation &amp; Artefakte

- Pflichtartefakte: Solution Design, ERD, Sequenz-/Integrationsdiagramme, Testplan, Runbook, Release Notes.

- Ablage in Git + Wissensbasis; Versionierung und Änderungslog.

1. KI-/GenAI-Nutzung (Abgleich mit 'KI Guardrails')

- Keine sensiblen Daten in externe Prompts; genehmigte Dienste (z.B. Einstein) verwenden.

- Mensch-im-Loop für geschäftskritische Entscheidungen; Nachvollziehbarkeit/Logging.

Nächste Schritte
- Abgleich mit euren bestehenden (falls vorhanden) Projekt-Guardrails.

- Lücken/Abweichungen dokumentieren und via CAB/ADR beschließen.

- Verantwortliche und Review-Kadenz (z.B. quartalsweise) festlegen.