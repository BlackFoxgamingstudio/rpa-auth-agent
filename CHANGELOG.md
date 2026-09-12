# Changelog — Sovereign RPA Auth Agent

All notable changes follow [Keep a Changelog](https://keepachangelog.com/en/1.1.0/) format.
Versioning follows [Semantic Versioning](https://semver.org/).

## [Unreleased]

### Added
- Full ecosystem documentation suite (ARCHITECTURE, DEVELOPER_GUIDE, SME_PLAYBOOK, SOP)
- GitHub Actions CI matrix (Python 3.10 / 3.11 / 3.12)
- Multistage Dockerfile with non-root user, health check, OCI labels
- docker-compose.yml with SBB platform network
- n8n custom node integration via `SovereignTools`
- `.env.example` environment template
- CONTRIBUTING, CODE_OF_CONDUCT, SECURITY governance files
- OpenAPI 3.1 compatible REST API spec
- Bandit security scan in CI

## [1.0.0] — 2024-01-01

### Added
- Initial production release of Sovereign RPA Auth Agent (PKG-010)
- Core microservice on port `8788`
- n8n webhook adapter (`n8n/webhook_adapter.py`)
- REST API (`POST /api/v1/execute`, `GET /health`)
- Components: BrowserAutomator, AuthSessionManager, MCPToolRunner, AuditLogger, RetryOrchestrator
- pyproject.toml packaging with `[dev]` extras
- CLI: `sovereign-rpa-auth-agent --help`
- Unit test suite (pure `unittest.TestCase`, no external test framework required)

### Domain: Robotic Process Automation & MCP
Authenticated RPA agent for automating legacy web portals, ERPs, and desktop apps. Integrates with MCP for tool use, session management, and audit logging.

[Unreleased]: https://github.com/BlackFoxgamingstudio/rpa-auth-agent/compare/v1.0.0...HEAD
[1.0.0]: https://github.com/BlackFoxgamingstudio/rpa-auth-agent/releases/tag/v1.0.0
