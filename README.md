# CodeAlpha Jenkins Remoting

Distributed Jenkins CI/CD setup demonstrating **Jenkins Remoting** — delegating build workloads from a Jenkins Controller to a remote agent node via JNLP4-connect protocol. Entire stack runs in Docker containers.

## Architecture

```
Jenkins Controller (port 8080)  ←→  Remote Agent (JNLP port 50000)
    jenkins-controller               jenkins-agent (remote-agent-1)
```

## Stack

- **Jenkins LTS** — CI/CD controller with JCasC
- **Jenkins Remoting** — agent-controller communication
- **Docker Compose** — multi-service orchestration
- **Declarative Pipeline** — 4-stage build workflow

## Quick Start

```bash
docker-compose up -d --build
```

- Jenkins UI: `http://localhost:8080` (admin / admin)

## Structure

```
├── docker-compose.yml          # Controller + agent services
├── jenkinsfile                 # Declarative pipeline
├── .env                        # Agent secret token
├── controller/
│   ├── Dockerfile              # Jenkins LTS + JCasC plugins
│   └── jenkins.yaml            # Configuration as Code
└── agent/
    └── Dockerfile              # Inbound agent with build tools
```

## Pipeline Stages

1. **Checkout** — clone from SCM
2. **Build** — execute on remote agent
3. **Test** — run tests
4. **Archive** — save build artifacts

## Credentials

Default admin login: `admin` / `admin` (configured via JCasC).
