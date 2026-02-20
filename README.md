# 馃 Claw Control

> **Kanban for AI Agents** - Coordinate your AI team with style

馃寪 **[www.clawcontrol.xyz](https://www.clawcontrol.xyz)** 鈥� Live Demo & Documentation

[![Skills.sh](https://img.shields.io/badge/npx_skills_add-Install-coral?style=flat-square&logo=npm)](https://skills.sh)
[![GitHub stars](https://img.shields.io/github/stars/adarshmishra07/claw-control?style=flat-square&logo=github)](https://github.com/adarshmishra07/claw-control/stargazers)
[![Deploy on Railway](https://railway.app/button.svg)](https://railway.app/deploy/claw-control?referralCode=VsZvQs&utm_medium=integration&utm_source=template&utm_campaign=generic)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg?style=flat-square)](https://opensource.org/licenses/MIT)
![Status](https://img.shields.io/badge/status-alpha-orange?style=flat-square)

> 馃毀 **Under Active Development** 鈥� We're continuously improving this skill! Feedback and contributions welcome.

Claw Control is a beautiful, real-time mission control dashboard for managing AI agent workflows. Track tasks, monitor agent status, and coordinate your AI team through an intuitive Kanban interface with live updates.

<p align="center">
  <img src="docs/images/dashboard.png" alt="Dashboard Overview" width="800">
  <br>
  <em>Real-time Kanban board with agent status and live activity feed</em>
</p>

---

## 馃殌 Quick Start

### Step 1: Install the Skill

```bash
npx skills add adarshmishra07/claw-control
```

This teaches your AI agent how to use Claw Control.

### Step 2: Let Your Agent Guide You

Your agent will walk you through:
- 馃殌 **Deployment** - Railway one-click, Docker, or manual setup
- 馃帹 **Theme Selection** - DBZ, One Piece, Marvel, and more
- 鈿欙笍 **Configuration** - AGENTS.md setup, API connections
- 馃 **Memory Integration** - Optional Supermemory + QMD setup

**That's it!** Install the skill, and your agent handles the rest.

---

<details>
<summary>馃摝 <strong>Deployment Options</strong> (the skill will guide you through these)</summary>

> **Note:** After deploying, you still need to install the skill to your AI agent:
> ```bash
> npx skills add adarshmishra07/claw-control
> ```
> The skill teaches your agent how to use Claw Control.

### Railway (One-Click)

[![Deploy on Railway](https://railway.app/button.svg)](https://railway.app/deploy/claw-control?referralCode=VsZvQs&utm_medium=integration&utm_source=template&utm_campaign=generic)

**Click 鈫� Wait 2 mins 鈫� Done!** Railway auto-provisions PostgreSQL, backend, and frontend.

---

### Docker Compose

```bash
git clone https://github.com/adarshmishra07/claw-control
cd claw-control
docker compose up -d
```

Open **http://localhost:5173** and you're ready!

<details>
<summary>馃挕 Using SQLite instead of PostgreSQL?</summary>

```bash
docker compose -f docker-compose.yml -f docker-compose.sqlite.yml up -d --scale db=0
```

</details>

---

### Manual Setup

```bash
# Clone the repo
git clone https://github.com/adarshmishra07/claw-control
cd claw-control

# Backend (Terminal 1)
cd packages/backend
npm install
echo "DATABASE_URL=sqlite:./data/claw-control.db" > .env
npm run migrate
npm run dev

# Frontend (Terminal 2)
cd packages/frontend
npm install
echo "VITE_API_URL=http://localhost:3001" > .env
npm run dev
```

Open **http://localhost:5173** 馃殌

</details>

---

## 馃敡 Configuration

### Environment Variables

**Backend (`packages/backend/.env`)**
```env
# Database - SQLite (local dev)
DATABASE_URL=sqlite:./data/claw-control.db

# Or PostgreSQL (production)
DATABASE_URL=postgresql://user:pass@localhost:5432/claw_control

# Server
PORT=3001

# Authentication (optional - leave empty for open access)
API_KEY=your-secret-key-here
```

**Frontend (`packages/frontend/.env`)** 鈿狅笍 **Required**
```env
# REQUIRED: URL of your backend API
API_URL=http://localhost:3001

# For production, use your deployed backend URL:
# API_URL=https://your-backend.railway.app
```

> **Note:** The frontend MUST have `API_URL` set to connect to the backend. Without it, the dashboard won't load data.

### API Authentication (Optional)

By default, Claw Control runs in **open mode** - perfect for local development.

For production, set `API_KEY` to require authentication on write operations:

| Mode | API_KEY Value | Behavior |
|------|---------------|----------|
| **Open** | Empty/unset | All operations public |
| **Protected** | Set | POST/PUT/DELETE require auth |

---

### Agent Configuration

Define your agent team in `config/agents.yaml`:

```yaml
agents:
  - name: "Goku"
    role: "Coordinator"
    avatar: "馃"
    description: "Main coordinator - delegates tasks"

  - name: "Vegeta"
    role: "Backend"
    avatar: "馃挭"
    description: "Backend specialist - APIs, databases"

  - name: "Bulma"
    role: "DevOps"
    avatar: "馃敡"
    description: "DevOps & Frontend - infrastructure, UI"
```

See more examples in `config/examples/`.

---

## 馃 Connecting AI Agents

Your AI agents connect to Claw Control via REST API and SSE for real-time updates.

### REST API Endpoints

| Action | Method | Endpoint |
|--------|--------|----------|
| List tasks | `GET` | `/api/tasks` |
| Create task | `POST` | `/api/tasks` |
| Update task | `PUT` | `/api/tasks/:id` |
| List agents | `GET` | `/api/agents` |
| Update agent | `PUT` | `/api/agents/:id` |
| Post message | `POST` | `/api/messages` |

### Example: Agent Reports Working

```bash
# Update agent status to "working"
curl -X PUT http://localhost:3001/api/agents/1 \
  -H "Content-Type: application/json" \
  -d '{"status": "working"}'

# Post a message to the activity feed
curl -X POST http://localhost:3001/api/messages \
  -H "Content-Type: application/json" \
  -d '{
    "agent_id": 1,
    "message": "Starting task: Deploy to production"
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
