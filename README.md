# n8n + SQL Server with Docker Compose

An n8n workflow setup with a SQL Server 2022 Developer edition database, both running locally in Docker.

## What's in it

| Service | Container name | Port |
|---|---|---|
| n8n (workflow automation) | `n8n` | 5678 |
| SQL Server 2022 Developer edition | `sql1` | 1433 |

n8n runs the workflows and talks to SQL Server over the Docker network created by Compose. All data is stored in Docker volumes (`n8n_data` and `sqldata`), so it survives container restarts.

## Prerequisites

- Docker and Docker Compose

Tested on Ubuntu.

## Setup

1. Clone the repo and go into the folder.
2. Create the SQL Server data volume (it is declared as `external` in `compose.yaml`, so it must exist first):
   ```bash
   docker volume create sqldata
   ```
3. Copy the example env file:
   ```bash
   cp .env.example .env
   ```
4. Open `.env` and set your own `MSSQL_SA_PASSWORD`. It must contain upper and lower case letters, numbers, and symbols, or the SQL Server container will exit immediately.
5. Start the containers:
   ```bash
   docker compose up -d
   ```
6. Check that both are running (SQL Server takes about 30 seconds to become `healthy`):
   ```bash
   docker compose ps
   ```

## Usage

- n8n: http://localhost:5678
- SQL Server: `localhost`, port `1433`, user `sa` (from a client such as DBeaver)

## Connecting n8n to SQL Server

In n8n, create a **Microsoft SQL** credential with:

| Field | Value |
|---|---|
| Server | `sql1` |
| Port | `1433` |
| Database | `master` (or your own database) |
| User / Password | `sa` and the password from your `.env` |
| Domain | leave empty |
| Ignore SSL Issues | on (local development only) |

Use `sql1` as the server, not `localhost`. Inside n8n's container, `localhost` points at n8n itself, while `sql1` is the SQL Server container's name on the shared Docker network.

## Notes

- Never commit `.env`. It is listed in `.gitignore`.
- "Ignore SSL Issues" is only acceptable for local development, because the container uses a self-signed certificate.
- Don't run `docker compose down -v`. The `-v` flag deletes the volumes and all your data.
- To stop the containers and keep your data, run `docker compose down`.# n8n + SQL Server with Docker Compose

An n8n workflow setup with a SQL Server 2022 Developer edition database, both running locally in Docker.

## What's in it

| Service | Container name | Port |
|---|---|---|
| n8n (workflow automation) | `n8n` | 5678 |
| SQL Server 2022 Developer edition | `sql1` | 1433 |

n8n runs the workflows and talks to SQL Server over the Docker network created by Compose. All data is stored in Docker volumes (`n8n_data` and `sqldata`), so it survives container restarts.

## Prerequisites

- Docker and Docker Compose

Tested on Ubuntu.

## Setup

1. Clone the repo and go into the folder.
2. Create the SQL Server data volume (it is declared as `external` in `compose.yaml`, so it must exist first):
   ```bash
   docker volume create sqldata
   ```
3. Copy the example env file:
   ```bash
   cp .env.example .env
   ```
4. Open `.env` and set your own `MSSQL_SA_PASSWORD`. It must contain upper and lower case letters, numbers, and symbols, or the SQL Server container will exit immediately.
5. Start the containers:
   ```bash
   docker compose up -d
   ```
6. Check that both are running (SQL Server takes about 30 seconds to become `healthy`):
   ```bash
   docker compose ps
   ```

## Usage

- n8n: http://localhost:5678
- SQL Server: `localhost`, port `1433`, user `sa` (from a client such as DBeaver)

## Connecting n8n to SQL Server

In n8n, create a **Microsoft SQL** credential with:

| Field | Value |
|---|---|
| Server | `sql1` |
| Port | `1433` |
| Database | `master` (or your own database) |
| User / Password | `sa` and the password from your `.env` |
| Domain | leave empty |
| Ignore SSL Issues | on (local development only) |

Use `sql1` as the server, not `localhost`. Inside n8n's container, `localhost` points at n8n itself, while `sql1` is the SQL Server container's name on the shared Docker network.

## Notes

- Never commit `.env`. It is listed in `.gitignore`.
- "Ignore SSL Issues" is only acceptable for local development, because the container uses a self-signed certificate.
- Don't run `docker compose down -v`. The `-v` flag deletes the volumes and all your data.
- To stop the containers and keep your data, run `docker compose down`.
