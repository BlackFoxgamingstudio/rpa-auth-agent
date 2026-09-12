# Standard Operating Procedure: Sovereign Rpa Auth Agent

## 1. Service Health Verification
Run `sovereign-rpa-auth-agent --health` to confirm the engine responds with status `HEALTHY`.

## 2. Webhook Adapter Operation
The webhook adapter listens on port `8788`:
```bash
python3 n8n/webhook_adapter.py
```
If port 8788 is occupied, check active processes:
```bash
lsof -i :8788
```

## 3. n8n Integration Testing
Send a probe POST request:
```bash
curl -X POST http://localhost:8788/api/v1/execute \
  -H "Content-Type: application/json" \
  -d '{"action": "health_ping", "payload": {"test": true}}'
```
