# Windmill CLI Quickstart

[`wmill`](https://www.windmill.dev/docs/advanced/cli) is the official command
line interface for [Windmill](https://www.windmill.dev) — an open-source
platform for internal tools, workflows, API integrations, background jobs, and
UIs. Use it to authenticate against a workspace, scaffold local projects,
sync scripts/flows/apps between your filesystem and a workspace, and run or
debug jobs from your terminal.

## Install

```sh
npm install -g windmill-cli
wmill --version
```

Upgrade later with `wmill upgrade`.

## Connect to a workspace

```sh
wmill workspace add
```

This walks you through adding a workspace profile — a `(name, remote URL,
workspace id, token)` tuple stored under `~/.config/windmill`. You can have
multiple profiles and switch between them with `wmill workspace switch <name>`.

A workspace token is created from the Windmill UI under
`User Settings → Tokens`. For self-hosted instances, point the remote at your
own URL (e.g. `https://windmill.example.com`).

## Initialize a project directory

```sh
wmill init
```

`wmill init` creates:

- `wmill.yaml` — sync configuration (which folders/types to track).
- `AGENTS.md` + `CLAUDE.md` — the agent prompt published in this repo.
- `.claude/skills/` and `.agents/skills/` — per-task guides used by AI coding
  assistants (Claude Code, Codex, Pi). These are the same `SKILL.md` files
  you'll find under `skills/` in this repo.

It also offers to bind a workspace profile to the current git branch and to
import git-sync settings from the backend if any are configured.

## Sync between local files and a workspace

```sh
wmill sync pull     # workspace → local (writes flows, scripts, apps, etc.)
wmill sync push     # local → workspace
```

Sync is idempotent and diff-aware: `wmill sync push --dry-run` previews the
changes without applying them. Use `--yaml` (recommended) to keep specs as
YAML rather than JSON.

For individual entities you can also use the type-specific commands:

```sh
wmill script push   path/to/script.ts
wmill flow   push   path/to/flow.yaml
wmill app    push   path/to/app.yaml
wmill resource push path/to/resource.yaml
```

## Run, inspect, and debug jobs

```sh
wmill script run u/me/my_script --data '{"foo": "bar"}'
wmill flow   run u/me/my_flow   --data @inputs.json
wmill job list --failed --limit 20
wmill job get  <job_id>
wmill job logs <job_id>
```

Logs and flow steps stream as the job runs. For flow failures, `wmill job get`
shows the step tree with each sub-job's id so you can drill in with
`wmill job logs <sub_job_id>`.

## Scaffold new entities

```sh
wmill script new u/me/path --language bun
wmill flow   new u/me/path --summary "..."
wmill app    new u/me/path --summary "..." --framework svelte
```

These create the correct folder layout and a minimal spec file, then print
next-step hints. Prefer them over hand-creating the folders — they pick the
right naming conventions for your workspace.

## Triggers and schedules

Triggers (HTTP routes, WebSocket, Kafka, NATS, MQTT, SQS, GCP Pub/Sub, Azure
Event Hubs, Email, Postgres CDC) and cron schedules are tracked as YAML files
synced alongside your scripts and flows. See `skills/triggers/SKILL.md` and
`skills/schedules/SKILL.md` for the full schemas.

## Completion

```sh
source <(wmill completions bash)        # bash, zsh: source <(wmill completions zsh)
source (wmill completions fish | psub)  # fish
```

## Reference

- `cli-commands.md` — every `wmill` command and flag, generated from the
  source.
- `AGENTS.md` — the top-level prompt the CLI installs into each project (and
  the same instructions AI coding assistants follow when working in a
  Windmill repo).
- `skills/<name>/SKILL.md` — one self-contained guide per common task.

### Skills index

- `skills/write-script-ansible/SKILL.md`
- `skills/write-script-bash/SKILL.md`
- `skills/write-script-bigquery/SKILL.md`
- `skills/write-script-bun/SKILL.md`
- `skills/write-script-bunnative/SKILL.md`
- `skills/write-script-csharp/SKILL.md`
- `skills/write-script-deno/SKILL.md`
- `skills/write-script-duckdb/SKILL.md`
- `skills/write-script-go/SKILL.md`
- `skills/write-script-graphql/SKILL.md`
- `skills/write-script-java/SKILL.md`
- `skills/write-script-mssql/SKILL.md`
- `skills/write-script-mysql/SKILL.md`
- `skills/write-script-php/SKILL.md`
- `skills/write-script-postgresql/SKILL.md`
- `skills/write-script-powershell/SKILL.md`
- `skills/write-script-python3/SKILL.md`
- `skills/write-script-rlang/SKILL.md`
- `skills/write-script-rust/SKILL.md`
- `skills/write-script-snowflake/SKILL.md`
- `skills/write-flow/SKILL.md`
- `skills/raw-app/SKILL.md`
- `skills/triggers/SKILL.md`
- `skills/schedules/SKILL.md`
- `skills/resources/SKILL.md`
- `skills/write-workflow-as-code/SKILL.md`
- `skills/cli-commands/SKILL.md`
- `skills/preview/SKILL.md`

## About this repo

Auto-generated mirror of the Windmill CLI's bundled AI-agent guidance and
command reference, published for ingestion by docs aggregators such as
[context7](https://context7.com).

**Do not edit by hand.** This repo is regenerated from
[windmill-labs/windmill](https://github.com/windmill-labs/windmill) on every
release. Open issues and PRs in the source repo, not here. The generator is
`system_prompts/generate.py --context7-dir`.
