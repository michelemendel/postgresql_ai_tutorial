# PostgreSQL AI Tutorial

Learning PostgreSQL by leaning on AI assistants instead of traditional textbooks or video courses. This repo tracks the prompts we use, the study plan generated from them, and the lightweight local environment needed to experiment.

## Goals

- Build a repeatable study workflow that relies on ChatGPT, Perplexity, and Warp for guidance, research, and exploratory questions.
- Focus on skills relevant to running PostgreSQL in Docker locally while preparing for a managed PostgreSQL deployment later.
- Keep all guidance/versioned notes in Markdown so progress is easy to review or adapt.

## Repo Layout

### Folder Structure

- `docs/` – all Markdown guidance (overview, local setup, classic tutorials).
- `prompts/` – AI prompt definitions.
- `migrations/` – SQL files consumed by the `migrate` service (`init.sql`, etc.).
- `learnings/` – notes and insights from interactive study sessions.
- project root – runtime files (`docker-compose.yml`, `.env`, `.env.local`, README).

### Key Files

- `docs/overview.md` – high-level plan, key links, and focus topics.
- `docs/local_setup.md` – Docker Compose instructions (Postgres 17, `.env` driven).
- `docs/classic_tutorials.md` – list of traditional resources (ignored by AI agents unless explicitly requested).
- `docs/study_plan.md` – study plan generated from `prompts/create_plan.md` (paste ChatGPT response here).
- `learnings/` – folder for notes and insights from interactive, non-linear study sessions (create individual Markdown files as needed).
- `prompts/create_plan.md` – master prompt used to generate/refresh the learning roadmap.
- `.env.example`, `.env.local`, `docker-compose.yml` – local runtime configuration.

## Tooling

- **ChatGPT**: primary planning and deep-dive questions (prompt stored in `prompts/create_plan.md`).
- **Perplexity**: topic research and cross-checking facts.
- **Warp** / **Cursor** / **Antigravity**: interactive shells/IDEs with inline AI to try queries, capture answers, and keep the learning loop tight.

### How the agent uses docs & prompts

- The agent treats files under `docs/` as the canonical narrative (overview, setup, classic resources). Reference them by filename or section when giving instructions; no need to paste the content into chat unless you are pointing out a specific snippet.
- `prompts/create_plan.md` is the authoritative master prompt. When you want the agent to regenerate the study plan, mention this file ("run the plan from `prompts/create_plan.md`") so it knows exactly which instructions to follow. Paste the response into `docs/study_plan.md`.
- Document interactive learnings (from Warp, Cursor, etc.) by creating files in `learnings/` as you discover new concepts.
- For files marked as "ignore" (e.g., `docs/classic_tutorials.md`), explicitly tell the agent if you want to override that behavior.

## Contributing Workflow

1. Update prompts or docs in `docs/` as you iterate on the learning plan.
2. When generating a new study plan from `prompts/create_plan.md`, paste the ChatGPT response into `docs/study_plan.md`.
3. Document learnings from interactive sessions (Warp, Cursor, etc.) by creating files in `learnings/`.
4. Capture other noteworthy AI outputs (from Perplexity, ChatGPT deep-dives, etc.) as new Markdown files in `learnings/`.
5. Run `docker compose down` when finished hacking to keep the environment clean.
6. Commit and push changes once the docs/environment updates are in place.

## Getting Started

1. Copy `.env.example` to `.env.local` (keep `.env.local` untracked or customize credentials there).
2. Start PostgreSQL: `docker compose up -d`.
3. Run migrations when needed: `docker compose run --rm migrate`.
4. Connect with `pgcli` or `psql` using the credentials defined in `.env.local`.

Aliases for convenience (add to your shell RC):

```
alias pgdev-start='docker compose up -d'
alias pgdev-stop='docker compose down'
alias pgdev-migrate='docker compose run --rm migrate'
alias pgdev-reset='docker compose down -v'
# Set environment variables from .env file
alias pgsetenv='set -a; source .env.local; set +a'
# Connect with psql
alias pgconnect='psql -h localhost -p 5432 -U "${POSTGRES_USER}" -d "${POSTGRES_DB}"'
# Connect with pgcli
alias pgcliconnect='pgcli -h localhost -p 5432 -U "${POSTGRES_USER}" -d "${POSTGRES_DB}"'

```

### Using `pgsetenv`

The `pgsetenv` alias exports environment variables from `.env.local` into your current shell session. Add this to your `~/.zshrc`:

```bash
alias pgsetenv='set -a; source .env.local; set +a'
```

Run `pgsetenv` in your terminal before using `pgconnect` or `pgcliconnect` aliases.

### Using `pgconnect` / `pgcliconnect`

- Both aliases live in `~/.zshrc` and rely on the environment variables `POSTGRES_USER` and `POSTGRES_DB` being exported in your shell session.
- Before using the aliases, run `pgsetenv` in your terminal to export the variables from `.env.local`.
- Then run `pgconnect` (psql) or `pgcliconnect` (pgcli); the alias will substitute `${POSTGRES_USER}`/`${POSTGRES_DB}` automatically using the values from your shell environment.
