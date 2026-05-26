# CodeAlpha Jenkins Remoting

Dockerized Jenkins Controller + Agent setup using JNLP (inbound) remoting.

## Architecture

```
┌─────────────────────┐       JNLP (port 50000)       ┌──────────────────┐
│  Jenkins Controller │◄──────────────────────────────►│  Jenkins Agent   │
│  (port 8080 - UI)   │        jenkins-net             │  (remote-agent)  │
└─────────────────────┘                                └──────────────────┘
```

## Quick Start

### 1. Build & Start

```bash
docker compose up --build
```

### 2. Access Jenkins

Open **http://localhost:8080** in your browser.

- **Username:** `admin`
- **Password:** `admin`

### 3. Connect the Agent

After the controller is healthy:

1. Go to **Manage Jenkins → Nodes → remote-agent**
2. Copy the **agent secret**
3. Paste it in the `.env` file:
   ```
   JENKINS_AGENT_SECRET=your_secret_here
   ```
4. Restart the agent:
   ```bash
   docker compose restart jenkins-agent
   ```

## Project Structure

```
CodeAlpha_Jenkins_Remoting/
├── controller/
│   ├── Dockerfile          # Jenkins controller image
│   └── jenkins.yaml        # Configuration as Code (JCasC)
├── agent/
│   └── Dockerfile          # Jenkins inbound agent image
├── docker-compose.yml      # Orchestration
├── .env                    # Agent secret (gitignored)
└── README.md
```

## Commands

| Command | Description |
|---------|-------------|
| `docker compose up --build` | Build & start all services |
| `docker compose up -d` | Run in background |
| `docker compose down` | Stop all services |
| `docker compose logs -f` | View live logs |
| `docker compose ps` | List running containers |

## Ports

| Service | Port | Description |
|---------|------|-------------|
| Controller | 8080 | Jenkins Web UI |
| Controller | 50000 | Agent remoting (JNLP) |
