# Local setup using docker compose

## Command line tools

### Install psql

(this makes it possible to use psql on macOS without installing the full PostgreSQL server package)

1. brew install libpq
2. brew link --force libpq

### Install pgcli

brew install pgcli

## Local Postgres on macOS using Docker (project-local)

### Goal:

- Run PostgreSQL locally using Docker, not a local installation.
- Keep `docker-compose.yml` inside this repository so everything runs from the project root.
- Use a named volume for DB data.
- Use a separate migration service so migrations are not tied to container startup.
- Load credentials and configuration from a `.env` file that lives next to the compose file.

## 1. Create the Compose file

See `./docker-compose.yml`:

Notes:

- Relative paths like ./migrations are relative to the location of the compose file, not your current working directory.
- The db service runs Postgres normally without overriding its CMD.
- The migrate service waits for Postgres, runs the SQL script, then exits.

## 2. Create the `.env.local` file

Copy `.env.example` to `.env.local` and customize if needed:

    cp .env.example .env.local

The `.env.local` file contains your database credentials and is not tracked in version control.

## 3. Create the migrations folder and an example SQL file

Create:

    mkdir -p ./migrations

Create example init.sql:

    cat > ./migrations/init.sql <<'SQL'
    CREATE TABLE IF NOT EXISTS users (
      id serial PRIMARY KEY,
      name text NOT NULL
    );
    SQL

## 4. Start and stop Postgres (run from repo root)

Start in background:

    docker compose up -d

Stop:

    docker compose down

Run migrations on demand:

    docker compose run --rm migrate

## 5. Optional wrapper commands (no scripts required but useful)

Add these to your shell rc file to avoid repeating flags:

    alias pgdev-start='docker compose up -d'
    alias pgdev-stop='docker compose down'
    alias pgdev-migrate="docker compose run --rm migrate"
    alias pgdev-reset='docker compose down -v'

## 6. Connect with psql

Using psql installed via brew:

    psql -h localhost -p 5432 -U "${POSTGRES_USER}" -d "${POSTGRES_DB}"
    # password from .env.local

From inside the container:

    docker exec -it pgdev psql -U "${POSTGRES_USER}" -d "${POSTGRES_DB}"

## 7. Why this setup is preferred

- Compose file lives with the codebase, so relative path mounts are predictable.
- Named volume pgdata stores Postgres data reliably and survives container recreation.
- Migrations do not require restarting Postgres and do not override the default entrypoint.
- `.env.local` keeps credentials out of version control while still being easy to override per developer.

## 8. Optional improvements

- You can add COMPOSE_PROJECT_NAME=pgdev in your environment or call compose with -p pgdev.
- You can parameterize the migration directory using environment variables if you ever need project-specific migrations.

```

```
