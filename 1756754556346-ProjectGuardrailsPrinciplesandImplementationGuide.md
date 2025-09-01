# Project Guardrails: Principles and Implementation Guide
V2
## Overview
Guardrails are concise, enforceable constraints that keep the project aligned with strategy, safety, compliance, and delivery goals while preserving team autonomy. They clarify what is "allowed", "requires approval", and "disallowed".
## Scope and Objectives
- Align decisions with project vision and non-goals

- Reduce risk (security, privacy, reliability, cost)

- Accelerate delivery via clear boundaries and automation

- Enable consistent, auditable decisions

## Guardrail Categories
- Strategic &amp; Product
Vision, non-goals, target users, out-of-scope features

- Accessibility standards (e.g., WCAG 2.2 AA), UX patterns to reuse/avoid

Technical Architecture
- Approved languages/runtimes; preferred frameworks

- API style (REST/gRPC/GraphQL), versioning, breaking-change policy

- Cloud regions; IaC required; secrets management

Security &amp; Privacy
- Data classification and handling; encryption in transit/at rest

- Authentication/authorization patterns; SSO; secret rotation

- Third-party review; SBOM; vulnerability SLAs

Data
- Single source of truth; schema evolution; PII minimization

- Retention windows; lineage; backup/restore RPO/RTO

Operational Reliability
- SLOs and error budgets; alerting thresholds

- Deployment strategies (blue/green, canary); rollback policy

Delivery Process
- Branching strategy; required reviews; test coverage minimums

- CI/CD gates; change management for production

Financial &amp; Vendor
- Cost budgets and alerts; tagging; FinOps reviews

- Preferred vendors; exit strategies; rate limits

Responsible AI (if applicable)
- Allowed data sources; prompt/content safety; human-in-the-loop

- Evaluation metrics; model/version registry; red-teaming

## Decision Rights and Exceptions
- Owners: Product, Engineering, Security, and Data leads co-own guardrails.

- Approval matrix:
"Allowed": team proceeds, self-serve

- "Requires approval": approval from the relevant owner(s) before merge/release

- "Disallowed": needs Steering Committee decision to change the guardrail

Exceptions:
- Time-boxed with expiry date, mitigations, monitoring, and rollback plan

- Logged in the Exceptions register and revisited at review cadence

## Implementation Steps
1. Identify top risks and pivotal decisions (architecture, data, vendors, compliance).

1. Author guardrails using a standard format: Rule, Rationale, Evidence (tests/policy), Owner, Exceptions process.

1. Map each guardrail to a measurable control and policy-as-code where feasible.

Embed and automate in SDLC:
- Templates: PR, issue, ADR

- CI gates: tests, security scans, IaC policy checks

- Branch protections and code ownership

1. Communicate and train; make guardrails visible in the repo and docs site.

1. Measure adherence; review quarterly; update as strategy and risk evolve.

## Lightweight Examples
### Policy-as-code (OPA/Rego) enforcing required cloud tags
package guardrails.tags

deny[msg] {
  input.resource.type == "aws_instance"
  not input.resource.tags["owner"]
  msg := "EC2 instances must include tag 'owner'"
}
### GitHub Actions CI gate (tests + SAST + policy check)
name: ci
on: [pull_request]
jobs:
  build-test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with: { node-version: '20' }
      - run: npm ci
      - run: npm test -- --coverage
  security-and-policy:
    runs-on: ubuntu-latest
    needs: build-test
    steps:
      - uses: actions/checkout@v4
      - name: SAST
        uses: github/codeql-action/analyze@v3
      - name: Policy check (Conftest)
        run: |
          curl -L -o conftest https://github.com/open-policy-agent/conftest/releases/latest/download/conftest_Linux_x86_64
          chmod +x conftest &amp;&amp; sudo mv conftest /usr/local/bin/
          conftest test iac/ --policy policy/
### CODEOWNERS to require approval on sensitive areas
# Sensitive data models require Data and Security review
data/**   @org/data-leads @org/security-team
infra/**  @org/platform-team
### ADR template referencing guardrails
# ADR-012: Choose message broker
Status: Proposed
Related guardrails: ARCH-API-03 (eventing), SEC-CRYPTO-02 (encryption), OPS-SLO-01 (latency)
Decision:
Consequences:
Evidence: load tests, threat model, POC link
Exception needed?: No
## Checklist
- Guardrails documented, versioned, and discoverable

- Each guardrail has an owner, rationale, measurable control, and test/policy

- CI/CD enforces critical controls; manual approvals only where necessary

- Exceptions process defined and logged

- Metrics and review cadence established

## Review Cadence and KPIs
- Cadence: quarterly review, or sooner after major incidents/regulatory change

- KPIs:
% PRs passing all guardrail checks on first try

- Mean time to approve exception requests

- Number of policy violations per release

- Cost and reliability trends vs. guardrail targets

## Appendix: Guardrail Record Template
- ID: ABC-AREA-NN

- Rule: One-sentence, testable constraint

- Rationale: Why this exists (risk, law, customer need)

- Evidence/Control: Tests, policies, dashboards

- Owner(s): Role or team

- Exceptions: Approval authority, expiry, mitigations

- Links: Standards, ADRs, runbooks