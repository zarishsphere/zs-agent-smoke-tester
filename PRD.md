# PRD — `zs-agent-smoke-tester`

> **Document Class:** PRD | **Version:** 1.0.0 | **Status:** Bootstrapping
> **Repository:** [https://github.com/zarishsphere/zs-agent-smoke-tester](https://github.com/zarishsphere/zs-agent-smoke-tester)
> **Layer:** Layer 9 — Agents & Automation | **Catalog #:** 199
> **License:** Apache 2.0 | **Governance:** RFC-0001

---

## 1. Overview

Smoke tester — post-deploy FHIR endpoint health checks and critical path tests.

---

## 2. Repository Metadata

- **Name:** `zs-agent-smoke-tester`
- **Organization:** [https://github.com/zarishsphere](https://github.com/zarishsphere)
- **Language / Runtime:** Go 1.26.1
- **Visibility:** Public
- **License:** Apache 2.0
- **Default Branch:** `main`
- **Branch Protection:** Required (2-owner review for critical paths)

---

## 3. Platform Context

This repository is part of the **ZarishSphere** sovereign digital health operating platform — a free, open-source, FHIR R5-native system for South and Southeast Asia.

**Non-negotiable constraints:**
- Zero cost — all tooling must use genuinely free tiers
- FHIR R5 native — all clinical data modelled as FHIR R5 resources
- Offline-first — must work without network connectivity
- No-coder friendly — GUI-first, template-driven
- Documentation as Code — all decisions in GitHub

---

## 4. Goals & Objectives

- Automate Smoke tester without human intervention
- Integrate with GitHub Actions and the GitHub API
- Run on free-tier infrastructure (Cloudflare Workers or GitHub Actions)

## 5. Functional Requirements

| ID | Requirement | Priority |
|----|------------|---------|
| F-01 | Core automation function fully implemented | P0 |
| F-02 | GitHub API integration (create issues, labels, comments) | P0 |
| F-03 | Error handling: never crash silently | P0 |
| F-04 | Test coverage >80% | P1 |
| F-05 | Configuration via GitHub Secrets / env vars | P0 |

## 6. Repository Tree

```
zs-agent-smoke-tester/
├── README.md
├── LICENSE
├── go.mod
├── go.sum
├── .gitignore
├── .github/
│   ├── CODEOWNERS
│   └── workflows/
│       └── ci.yml
├── cmd/
│   └── agent/
│       └── main.go
├── internal/
│   ├── smoke_tester/
│   │   └── smoke_tester.go
│   └── github/
│       └── client.go                  # GitHub API client wrapper
├── config/
│   └── config.go
└── tests/
    └── (unit + integration tests)
```

### CI/CD (`.github/workflows/ci.yml`)

```yaml
name: CI
on:
  push:
    branches: [main]
  pull_request:
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-go@v5
        with: { go-version-file: go.mod, cache: true }
      - run: go test ./... -race -coverprofile=coverage.out
      - uses: golangci/golangci-lint-action@v6
  security:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: aquasecurity/trivy-action@master
        with: { scan-type: fs, severity: CRITICAL,HIGH }
```

## 9. Ownership & Governance

| Role | GitHub User |
|------|-------------|
| Platform Lead | `@arwa-zarish` |
| Technical Lead | `@code-and-brain` |
| DevOps Lead | `@DevOps-Ariful-Islam` |
| Health Programs | `@BGD-Health-Program` |

All changes go through Pull Request → 1+ owner review → CI pass → merge.
Breaking changes require RFC + ADR.

---

## 10. Definition of Done

- [ ] All listed files exist with content
- [ ] CI pipeline passes (test + lint + security)
- [ ] README.md reflects current state
- [ ] OpenAPI / AsyncAPI spec present (services only)
- [ ] At least 1 integration test using testcontainers-go (Go) or Playwright (UI)
- [ ] No secrets committed (GitGuardian verified)
- [ ] CODEOWNERS file present
- [ ] Linked to CATALOGS.md and TODO.md

---

*This PRD is the canonical source of truth for this repository's purpose, structure, and requirements.*
*Changes require a PR against this file with owner approval.*
