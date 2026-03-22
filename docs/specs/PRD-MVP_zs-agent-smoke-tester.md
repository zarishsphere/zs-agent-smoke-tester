# PRD-MVP — `zs-agent-smoke-tester`

> **Document:** Product Requirements (MVP) | **Version:** 1.0.0-mvp
> **Repository:** [https://github.com/zarishsphere/zs-agent-smoke-tester](https://github.com/zarishsphere/zs-agent-smoke-tester)
> **Layer:** Layer 9 — Agents | **Catalog #:** 199
> **Language:** Go 1.26.1 / GitHub Actions | **License:** Apache 2.0

---

## Executive Summary

**Post-deploy smoke tester — FHIR endpoint health checks and critical path validation.**

This document defines the **Minimum Viable Product (MVP)** scope for `zs-agent-smoke-tester` within the ZarishSphere sovereign digital health platform. It covers what must be built first, acceptance criteria, user stories, and the complete repository file structure.


### Platform Non-Negotiables (apply to every repository)

| Constraint | Rule |
|-----------|------|
| **Zero Cost** | All tooling, hosting, and services must use genuinely free tiers |
| **Open Source** | Apache 2.0 license; all code public |
| **FHIR R5 Native** | All clinical data modelled as FHIR R5 resources |
| **Offline-First** | Must function without network connectivity |
| **No-Coder Friendly** | GUI-first, template-driven, automatable |
| **Documentation as Code** | All decisions in GitHub via RFC/ADR |
| **Multi-tenant** | tenant_id scoping on all data operations |
| **HIPAA/GDPR** | AuditEvent on all PHI access; field-level encryption |

---

## Problem Statement

A successful Argo CD deployment does not guarantee the service is actually working. Smoke tests catch application-level failures that infrastructure checks miss.

## MVP Goals

1. Core automation function implemented and tested
2. GitHub API integration working
3. Deployable to Cloudflare Workers or GitHub Actions
4. Error handling: all failures logged and surfaced as GitHub notifications

## Triggers

- Argo CD sync completes (webhook)
- Manual trigger for staged rollouts

## Outputs

- FHIR CapabilityStatement endpoint returns 200
- Patient CRUD smoke test passes
- Auth endpoint returns 200
- Grafana alert fired if smoke test fails
- Argo CD rollback triggered on failure

## MVP Functional Requirements

| ID | Requirement | Acceptance Criteria | Priority |
|----|------------|---------------------|---------|
| M-01 | Core automation function works end-to-end | Manual trigger produces expected output | P0 |
| M-02 | GitHub API auth via GitHub App private key | API calls succeed with App JWT | P0 |
| M-03 | Unit tests cover >80% of logic | `go test -cover` shows >80% | P1 |
| M-04 | No silent failures — all errors logged | zerolog error logs appear on failure | P0 |
| M-05 | Configuration via env vars / secrets | No hardcoded values | P0 |

## MVP Complete Repository Tree

```
zs-agent-smoke-tester/
├── README.md
├── LICENSE
├── go.mod
├── go.sum
├── .gitignore
├── .github/CODEOWNERS
├── .github/workflows/ci.yml
├── cmd/smoker/main.go
├── internal/
│   ├── fhir/health_check.go
│   ├── fhir/patient_smoke_test.go
│   └── argocd/rollback.go
└── config/
    └── endpoints.yaml
```

---


## Owners & Governance

| Role | GitHub Handle | Responsibility |
|------|--------------|----------------|
| Platform Lead | `@arwa-zarish` | Final approval, RFC votes |
| Technical Lead | `@code-and-brain` | Architecture, Go/TS review |
| DevOps Lead | `@DevOps-Ariful-Islam` | CI/CD, infra, deployment |
| Health Programs | `@BGD-Health-Program` | Clinical content, country programs |

**PR Policy:** All changes via Pull Request. Minimum 1 owner review. CI must pass. No direct commits to `main`.


---

## MVP Acceptance Checklist

- [ ] All MVP files exist in repository with real content (not placeholders)
- [ ] CI pipeline passes on `main` branch
- [ ] No secrets, credentials, or PHI committed
- [ ] README.md reflects current state with setup instructions
- [ ] CODEOWNERS file present
- [ ] All MVP functional requirements verified manually or via automated tests
- [ ] Linked to `CATALOGS.md` and `TODO.md` in `zs-docs-platform`

---

*This document is the authoritative MVP specification for `zs-agent-smoke-tester`.*
*Changes require a Pull Request with at least 1 owner approval.*
