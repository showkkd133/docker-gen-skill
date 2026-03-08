# docker-gen-skill

A Claude Code skill that auto-detects your project's tech stack and generates optimized Dockerfile, docker-compose.yml, and .dockerignore configurations.

## What it does

This skill analyzes your project -- language, framework, package manager, and dependencies -- then generates production-ready Docker configurations with multi-stage builds, security best practices, and health checks.

### Supported Tech Stacks

| Language | Runtimes / Frameworks |
|----------|----------------------|
| TypeScript/JavaScript | Bun, Node.js, Next.js, Vite, Express, Fastify, Hono |
| Python | FastAPI, Flask, Django (pip, uv) |
| Go | Gin, Echo, Fiber, Chi, gRPC |
| Rust | Actix-web, Axum, Rocket |
| Java | Spring Boot, Quarkus (Maven, Gradle) |

### Supported Services

| Category | Services |
|----------|----------|
| Database | PostgreSQL, MySQL, MongoDB |
| Cache | Redis |
| Reverse Proxy | Nginx |
| Search | Elasticsearch, Meilisearch |
| Message Queue | RabbitMQ, Kafka |

### What it generates

- **Dockerfile** -- Multi-stage build with minimal base image, non-root user, health check
- **docker-compose.yml** -- Production config with detected dependency services
- **docker-compose.dev.yml** -- Development overrides with hot reload and debug ports
- **.dockerignore** -- Excludes secrets, build artifacts, and unnecessary files
- **.env.example** -- Template for required environment variables

## Installation

### Claude Code (recommended)

Add to your Claude Code settings (`~/.claude/settings.json`):

```json
{
  "skills": {
    "docker-gen": {
      "source": "github:showkkd133/docker-gen-skill"
    }
  }
}
```

Or copy `skills/docker-gen/SKILL.md` to `~/.claude/skills/docker-gen/SKILL.md`.

### Manual

1. Clone this repo
2. Copy the skill file:
   ```bash
   cp skills/docker-gen/SKILL.md ~/.claude/skills/docker-gen/SKILL.md
   ```

## Usage

In Claude Code, just say:

- "generate docker" / "生成 docker"
- "dockerize this project" / "容器化"
- "create docker compose" / "docker compose"

The skill will:
1. Detect your project's tech stack and dependencies
2. Generate an optimized multi-stage Dockerfile
3. Generate docker-compose.yml with required services
4. Generate development overrides (docker-compose.dev.yml)
5. Generate .dockerignore and .env.example
6. Validate with `docker compose config`

## Security Features

- Non-root user in all containers
- Minimal base images (Alpine / slim / distroless / scratch)
- No hardcoded secrets -- environment variable injection
- `.env` excluded from Docker build context
- Fixed image version tags (no `latest`)
- Multi-stage builds exclude build tools from production

## Development vs Production

| Aspect | Development | Production |
|--------|------------|------------|
| Base image | Full (with debug tools) | Alpine / distroless |
| Build stages | Single (fast rebuild) | Multi-stage (minimal size) |
| Source code | Volume mount (hot reload) | COPY into image |
| Ports | App + debugger | App only |
| Dependencies | All (including dev) | Production only |
| Restart policy | `no` | `unless-stopped` |

## License

MIT
