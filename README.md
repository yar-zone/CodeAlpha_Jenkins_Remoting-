# CodeAlpha Task 2 — Jenkins Remoting Project

![Jenkins](https://img.shields.io/badge/Jenkins-D24939?style=for-the-badge&logo=jenkins&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![JCasC](https://img.shields.io/badge/JCasC-Configured-brightgreen?style=for-the-badge)

> A distributed Jenkins setup using Jenkins Remoting — connecting a remote agent node to a Jenkins controller via Docker, with automated pipeline execution.

---

## 📌 Project Overview

This project demonstrates **Jenkins Remoting** — the ability to distribute build workloads across multiple machines (nodes). The Jenkins Controller delegates pipeline jobs to a connected remote agent, enabling:

- Distributed and parallel build execution
- Node isolation for improved security
- Scalable CI/CD infrastructure
- Remote job execution across different architectures

---

## 🏗️ Architecture

```
┌─────────────────────┐         JNLP / Port 50000        ┌──────────────────────┐
│  Jenkins Controller  │ ◄──────────────────────────────► │   Remote Agent Node   │
│   (Docker Container) │                                   │  (remote-agent-1)     │
│   localhost:8080     │                                   │  Runs pipeline jobs   │
└─────────────────────┘                                   └──────────────────────┘
         │
         │ JCasC (jenkins.yaml)
         ▼
   Auto-configured on startup
```

---

## 📁 Project Structure

```
CodeAlpha_Jenkins_Remoting/
├── agent/
│   └── Dockerfile          # Remote agent image
├── controller/
│   ├── Dockerfile          # Jenkins controller image
│   └── jenkins.yaml        # JCasC configuration
├── .env                    # Agent secret (not committed)
├── docker-compose.yml      # Full stack definition
├── Jenkinsfile             # Pipeline script
└── README.md
```

---

## ⚙️ Setup & Installation

### Prerequisites
- [Docker Desktop](https://www.docker.com/products/docker-desktop/) installed and running
- Ports `8080` and `50000` available

### 1. Clone the Repository
```bash
git clone https://github.com/YOUR_USERNAME/CodeAlpha_Jenkins_Remoting
cd CodeAlpha_Jenkins_Remoting
```

### 2. Start the Stack
```bash
docker-compose up -d --build
```

### 3. Get the Agent Secret
1. Open **http://localhost:8080** → login with `admin / admin`
2. Go to **Manage Jenkins → Nodes → remote-agent-1**
3. Copy the secret token shown on the page

### 4. Update `.env` File
```env
AGENT_SECRET=paste_your_secret_here
```

### 5. Connect the Agent
```bash
# Restart to apply the secret
docker-compose restart jenkins-agent
```

### 6. Verify
Go to **Manage Jenkins → Nodes** — `remote-agent-1` should show a 🟢 green status.

---

## 🚀 Running the Pipeline

1. Go to **Dashboard → CodeAlpha-Remoting**
2. Click **Build Now**
3. Click the build → **Console Output**

Expected output:
```
Running on remote-agent-1 in .../workspace/CodeAlpha-Remoting
=== CodeAlpha Jenkins Remoting ===
Build ID: 2
Node: remote-agent-1
All tests passed on remote node!
```

---

## 🔧 Key Configuration Files

### `docker-compose.yml`
Defines two services:
- `jenkins-controller` — Jenkins master on port 8080
- `jenkins-agent` — Inbound agent connecting via JNLP on port 50000

### `controller/jenkins.yaml` (JCasC)
Auto-configures Jenkins on startup:
- Admin credentials
- Security settings
- Remote agent node definition
- Remoting security enabled

### `Jenkinsfile`
Declarative pipeline with 4 stages:
| Stage | Description |
|-------|-------------|
| Checkout | Confirms execution on remote node |
| Build | Runs build commands on the agent |
| Test | Executes test scripts remotely |
| Archive | Saves build artifacts |

---

## 🔐 Security Features

- **Node Isolation** — builds run on the agent, not the controller
- **JNLP Secret Token** — agent authenticates with a unique secret
- **Remoting Security** — enabled via JCasC (`remotingSecurity: true`)
- **No Anonymous Access** — enforced via authorization strategy

---

## 📸 Screenshots

| Jenkins Dashboard | Remote Agent Online | Successful Pipeline |
|:-:|:-:|:-:|
| *(add screenshot)* | *(add screenshot)* | *(add screenshot)* |

---

## 🛑 Stopping the Stack

```bash
# Stop containers
docker-compose down

# Stop and remove volumes (full reset)
docker-compose down -v
```

---

## 👤 Author

**Your Name**  
CodeAlpha DevOps Internship  
[LinkedIn](https://linkedin.com/in/yourprofile) | [GitHub](https://github.com/yourusername)

---

## 📄 License

This project is part of the CodeAlpha Internship Program.