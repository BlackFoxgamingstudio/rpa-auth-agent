# Architecture: Sovereign RPA Auth Agent

## Overview

**Package ID:** `PKG-010`  
**Domain:** Robotic Process Automation & MCP  
**Microservice Port:** `8788`  
**n8n Webhook Path:** `rpa-auth-agent-trigger`  
**GitHub:** [BlackFoxgamingstudio/rpa-auth-agent](https://github.com/BlackFoxgamingstudio/rpa-auth-agent)

Authenticated RPA agent for automating legacy web portals, ERPs, and desktop apps. Integrates with MCP for tool use, session management, and audit logging.

---

## System Architecture

```
                     ┌──────────────────────────────────┐
                     │       Sovereign RPA Auth Agent      │
                     │       Port: 8788            │
                     ├──────────────┬───────────────────┤
   n8n Webhook ────▶ │  REST API    │   Core Engine     │
   HTTP POST         │  /api/v1/*   │   Dispatcher      │
                     └──────┬───────┴────────┬──────────┘
                            │                │
              ┌─────────────▼────────────────▼─────────┐
              │          Component Layer                 │
              │  BrowserAutomato | AuthSessionMana | MCPToolRunne  │
              └────────────────────────┬────────────────┘
                                       │
              ┌────────────────────────▼────────────────┐
              │      n8n Central Event Bus (:5678)       │
              └─────────────────────────────────────────┘
```

## Core Components

### `BrowserAutomator`
Handles all browserautomator operations. Exposes async methods callable from the core dispatcher.

### `AuthSessionManager`
Handles all authsession operations. Exposes async methods callable from the core dispatcher.

### `MCPToolRunner`
Handles all mcptoolrunner operations. Exposes async methods callable from the core dispatcher.

### `AuditLogger`
Handles all auditlogger operations. Exposes async methods callable from the core dispatcher.

### `RetryOrchestrator`
Handles all retryorchestrator operations. Exposes async methods callable from the core dispatcher.

---

## API Contract

All interactions follow the SBB standard envelope:

```http
POST /api/v1/execute
Content-Type: application/json
X-SBB-API-Key: <api-key>

{
  "action": "<operation>",
  "payload": {},
  "trace_id": "optional-uuid"
}
```

**Success Response (HTTP 200):**
```json
{
  "status": "success",
  "data": {},
  "trace_id": "...",
  "timestamp": "2025-01-01T00:00:00Z"
}
```

**Health Check:**
```http
GET /health
→ {"status": "healthy", "service": "sovereign-rpa-auth-agent", "port": 8788}
```

## Integration Matrix

| System | Protocol | Direction | Purpose |
|--------|----------|-----------|---------|
| n8n Event Bus (:5678) | HTTP POST | Outbound | Event forwarding |
| n8n Webhook | HTTP POST | Inbound | Trigger execution |
| SBB Codebase Vault (:8766) | HTTP | Outbound | Code analysis |
| SBB Patterns Bible (:8794) | HTTP | Outbound | Standards validation |
| External APIs | HTTPS | Outbound | Domain-specific data |

## Deployment Architecture

```yaml
# docker-compose excerpt
sovereign-rpa-auth-agent:
  image: sovereign-rpa-auth-agent:latest
  ports: ["8788:8788"]
  healthcheck:
    test: curl -f http://localhost:8788/health
    interval: 30s
```

## Security Model

| Control | Implementation |
|---------|---------------|
| Authentication | `X-SBB-API-Key` header (env: `SBB_API_KEY`) |
| Rate Limiting | 100 req/min per client IP |
| Input Validation | Pydantic models (strict mode) |
| Container Security | Non-root user (`appuser:1001`) |
| Secrets | Environment variables only (never hardcoded) |
| TLS | Terminate at reverse proxy (nginx/caddy) |

## Tags
`rpa`, `automation`, `mcp`, `auth`
