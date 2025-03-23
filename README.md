# OpenProject Multi-Container Docker Setup

This setup is intended for a production-like environment using Docker Compose with separate containers for OpenProject and PostgreSQL.

## Setup

1. Copy `.env.example` to `.env` and set your `OPENPROJECT_SECRET_KEY_BASE`.
2. From the project directory, run:

```bash
docker compose up -d
```

This will:
- Start OpenProject on port 8080
- Start a PostgreSQL container as the database backend
- Create volumes for persistent data

## Reverse Proxy (Forge)

Configure Nginx (via Forge) to point to OpenProject:

```
location / {
    proxy_pass http://localhost:8080;
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-Proto $scheme;
}
```

## Data

- OpenProject data: `./data` (mounted into container)
- PostgreSQL data: Docker volume `pgdata`

Make sure to back up these volumes regularly.
