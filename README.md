// ~/.clawdbot/exec-approvals.json
{
  "agents": {
    "main": {
      "allowlist": [
        { "pattern": "/usr/bin/npm", "lastUsedAt": 1706644800 },
        { "pattern": "/opt/homebrew/bin/git", "lastUsedAt": 1706644900 }
      ]
    }
  }
}    "message": "Starting task: Deploy to production"
  }'
```

### Example: Create a Task

```bash
curl -X POST http://localhost:3001/api/tasks \
  -H "Content-Type: application/json" \
  -d '{
    "title": "Fix login bug",
    "description": "Users getting 401 on valid tokens",
    "status": "todo",
    "priority": "high"
  }'
```

### With Authentication

If `API_KEY` is set, include it in write requests:

```bash
curl -X POST http://localhost:3001/api/tasks \
  -H "Authorization: Bearer your-api-key" \
  -H "Content-Type: application/json" \
  -d '{"title": "New task"}'
```

### SSE for Real-Time Updates

Connect to the event stream to receive live updates:

```bash
curl -N http://localhost:3001/api/stream
```

**Events emitted:**
- `task-created` / `task-updated` / `task-deleted`
- `agent-updated`
- `message-created`

### JavaScript Example

```javascript
// Update agent status
await fetch('http://localhost:3001/api/agents/1', {
  method: 'PUT',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({ status: 'working' })
});

// Listen for real-time updates
const eventSource = new EventSource('http://localhost:3001/api/stream');
eventSource.onmessage = (event) => {
  const data = JSON.parse(event.data);
  console.log('Update:', data);
};
```

### OpenClaw Integration Script

For OpenClaw/Claude agents, copy the helper script:

```bash
cp templates/scripts/update_dashboard.js scripts/
export CLAW_CONTROL_URL=https://your-backend.railway.app
```

Then agents can update status with:

```bash
node scripts/update_dashboard.js --agent "Bulma" --status "working" --message "Starting deployment"
```

馃摉 Full guide: [docs/openclaw-integration.md](docs/openclaw-integration.md)

---

## 鉁� Features

- **馃搵 Kanban Board** - Drag-and-drop task management with real-time sync
- **馃 Agent Tracking** - Monitor agent status (idle/working/error)
- **馃挰 Activity Feed** - Real-time agent message stream
- **馃攧 SSE Updates** - Live updates without polling
- **馃摫 Mobile Responsive** - Works on any device
- **馃帹 Cyberpunk UI** - Sleek, dark theme with glowing accents
- **馃攲 MCP Integration** - Native Model Context Protocol support
- **馃梽锔� Flexible Storage** - SQLite (dev) or PostgreSQL (prod)

---

## 馃彈锔� Architecture

```
鈹屸攢鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹�
鈹�                         CLIENTS                              鈹�
鈹溾攢鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹攢鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹攢鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹攢鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹�
鈹� Dashboard 鈹� AI Agents 鈹� MCP Tools 鈹�  External Webhooks     鈹�
鈹�  (React)  鈹� (REST API)鈹�  (stdio)  鈹�  (GitHub, etc.)        鈹�
鈹斺攢鈹€鈹€鈹€鈹€鈹攢鈹€鈹€鈹€鈹€鈹粹攢鈹€鈹€鈹€鈹€鈹攢鈹€鈹€鈹€鈹€鈹粹攢鈹€鈹€鈹€鈹€鈹攢鈹€鈹€鈹€鈹€鈹粹攢鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹攢鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹�
      鈹�           鈹�           鈹�                鈹�
      鈹斺攢鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹粹攢鈹€鈹€鈹€鈹€鈹攢鈹€鈹€鈹€鈹€鈹粹攢鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹�
                        鈹�
                鈹屸攢鈹€鈹€鈹€鈹€鈹€鈹€鈻尖攢鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹�
                鈹�   API Server   鈹�
                鈹�   (Fastify)    鈹�
                鈹溾攢鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹�
                鈹� 鈥� REST API     鈹�
                鈹� 鈥� SSE Stream   鈹�
                鈹� 鈥� Auth Layer   鈹�
                鈹斺攢鈹€鈹€鈹€鈹€鈹€鈹€鈹攢鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹�
                        鈹�
                鈹屸攢鈹€鈹€鈹€鈹€鈹€鈹€鈻尖攢鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹�
                鈹�   Database     鈹�
                鈹溾攢鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹�
                鈹� SQLite 鈹� Postgres 鈹�
                鈹斺攢鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹€鈹�
```

### Tech Stack

| Layer | Technology |
|-------|------------|
| **Frontend** | React 19, TypeScript, Vite, TailwindCSS |
| **Backend** | Node.js, Fastify 5, Server-Sent Events |
| **Database** | SQLite (dev) / PostgreSQL (prod) |
| **AI Integration** | MCP Server, REST API |
| **Deployment** | Docker, Railway |

---

## 馃摎 Full Documentation

- **[Railway Deployment Guide](docs/railway-template.md)** - Detailed Railway setup
- **[OpenClaw Integration](docs/openclaw-integration.md)** - Connect AI agents
- **[API Reference](http://localhost:3001/documentation)** - Swagger UI (when running locally)
- **[Contributing Guide](CONTRIBUTING.md)** - How to contribute

---

## 馃摝 Project Structure

```
claw-control/
鈹溾攢鈹€ packages/
鈹�   鈹溾攢鈹€ frontend/          # React + Vite + TailwindCSS
鈹�   鈹�   鈹溾攢鈹€ src/
鈹�   鈹�   鈹�   鈹溾攢鈹€ components/  # UI components
鈹�   鈹�   鈹�   鈹溾攢鈹€ hooks/       # Custom React hooks
鈹�   鈹�   鈹�   鈹斺攢鈹€ types/       # TypeScript types
鈹�   鈹�   鈹斺攢鈹€ package.json
鈹�   鈹�
鈹�   鈹斺攢鈹€ backend/           # Fastify + SQLite/PostgreSQL
鈹�       鈹溾攢鈹€ src/
鈹�       鈹�   鈹溾攢鈹€ server.js      # Main API server
鈹�       鈹�   鈹溾攢鈹€ db-adapter.js  # Database abstraction
鈹�       鈹�   鈹溾攢鈹€ mcp-server.js  # MCP integration
鈹�       鈹�   鈹斺攢鈹€ migrate.js     # DB migrations
鈹�       鈹斺攢鈹€ package.json
鈹�
鈹溾攢鈹€ config/                    # Configuration files
鈹�   鈹溾攢鈹€ agents.yaml            # Your agent definitions
鈹�   鈹斺攢鈹€ examples/              # Example configs
鈹溾攢鈹€ docker-compose.yml         # Full stack (PostgreSQL)
鈹溾攢鈹€ docker-compose.sqlite.yml  # SQLite override
鈹斺攢鈹€ templates/scripts/         # Integration scripts
```

---

## 馃 Contributing

We welcome contributions! Please see our [Contributing Guide](CONTRIBUTING.md) for details.

---

## 馃搫 License

MIT License - see [LICENSE](LICENSE) for details.

---

<p align="center">
  Made with 馃 by the <a href="https://github.com/adarshmishra07">OpenClaw</a> team
</p>
