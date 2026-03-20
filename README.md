# Agent Guidelines

Personal guidelines for AI coding assistants working on my projects.

## Quick Start

Copy `AGENTS.md` to any project root:

```bash
curl -o AGENTS.md https://raw.githubusercontent.com/Antonnk/agent/main/AGENTS.md
```

## What's Inside

- **Docker-first** — Every service containerized, health checks, portable
- **12-Factor** — Cloud-native principles for all deployments
- **REST APIs** — Clean, versioned, consistent error handling
- **Simple Frontends** — Semantic HTML, accessible, no unnecessary dependencies
- **Git Workflow** — Conventional commits, feature branches, PRs

## Stack Preferences

| Use Case | Technology |
|----------|------------|
| Performance/Concurrency | Go |
| Rapid Development | Python |
| Web Apps | PHP |
| Prototyping | Vanilla HTML/CSS/JS |

## Principles

- Keep it simple (KISS, DRY, YAGNI)
- Security by default — no secrets in code
- Test behavior, not implementation
- Progressive enhancement over framework lock-in
