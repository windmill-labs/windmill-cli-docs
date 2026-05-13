# Windmill AI Agent Instructions

You are a helpful assistant that can help with Windmill scripts, flows, apps, and resources management.

## Important Notes
- Every new entity MUST be created using the skills listed below.
- Every modification of an entity MUST be done using the skills listed below.
- User MUST be asked where to create the entity. It can be in its user folder, under u/{user_name} folder, or in a new folder, /f/{folder_name}/. folder_name and user_name must be provided by the user.

## Script Writing Guide

You MUST use the `write-script-<language>` skill to write or modify scripts in the language specified by the user. Use bun by default.
For Workflow-as-Code scripts, use the `write-workflow-as-code` skill.

## Flow Writing Guide

You MUST use the `write-flow` skill to create or modify flows.
When a new flow needs to be created, YOU run `wmill flow new <path>` yourself (with `--summary` and optional `--description`) to scaffold the folder and `flow.yaml`, then edit `flow.yaml` to fill in modules and schema. Do NOT scaffold the folder + yaml by hand and do NOT tell the user to run `wmill flow new`. If path or summary are missing from the user's request, ask via `AskUserQuestion` (one call, all missing fields) — never invent them. See the `write-flow` skill for the procedure.

## Raw App Development

You MUST use the `raw-app` skill to create or modify raw apps.
When a new app needs to be created, YOU run `wmill app new` yourself with `--summary`, `--path`, and `--framework` flags (and any other relevant flags). Do NOT ask the user to run it. If you don't have the values for those flags, ask the user via `AskUserQuestion` (one call, all missing fields) — never invent them. See the `raw-app` skill for the full procedure.

## Triggers

You MUST use the `triggers` skill to configure HTTP routes, WebSocket, Kafka, NATS, SQS, MQTT, GCP, Azure, Email, or Postgres CDC triggers.

## Schedules

You MUST use the `schedules` skill to configure cron schedules.

## Resources

You MUST use the `resources` skill to manage resource types and credentials.

## Visual Preview

You MUST use the `preview` skill any time the user wants to see/open/visualize/preview a flow, script, or app in the dev page — and after writing one, when offering visual verification. The skill picks between an MCP-embedded proxy (one named `launch.json` entry per target) and direct mode (URL handed to the user) based on what tools you have.

## CLI Reference

You MUST use the `cli-commands` skill to use the CLI.

## Debugging Jobs

When the user reports a script or flow failure, is investigating unexpected output, or asks why something ran the way it did, use the CLI to fetch job details before speculating. See the `cli-commands` skill for all flags.

- `wmill job list --script-path <path>` — recent runs of a specific script or flow
- `wmill job list --failed --limit 20` — recent failures across the workspace
- `wmill job get <id>` — status, timing, and (for flows) the step tree with sub-job IDs
- `wmill job logs <id>` — stdout/stderr; for flows, aggregates every step's logs
- `wmill job result <id>` — JSON result of a completed job
- `wmill job cancel <id>` — stop a running or queued job

For flow failures, start with `wmill job get <id>` to identify the failing step and its sub-job ID, then `wmill job logs <sub-job-id>` to drill in.

## Skills

For specific guidance, ALWAYS use the skills listed below.

- `.claude/skills/write-script-bash/SKILL.md` - MUST use when writing Bash scripts.
- `.claude/skills/write-script-bigquery/SKILL.md` - MUST use when writing BigQuery queries.
- `.claude/skills/write-script-bun/SKILL.md` - MUST use when writing Bun/TypeScript scripts.
- `.claude/skills/write-script-bunnative/SKILL.md` - MUST use when writing Bun Native scripts.
- `.claude/skills/write-script-csharp/SKILL.md` - MUST use when writing C# scripts.
- `.claude/skills/write-script-deno/SKILL.md` - MUST use when writing Deno/TypeScript scripts.
- `.claude/skills/write-script-duckdb/SKILL.md` - MUST use when writing DuckDB queries.
- `.claude/skills/write-script-go/SKILL.md` - MUST use when writing Go scripts.
- `.claude/skills/write-script-graphql/SKILL.md` - MUST use when writing GraphQL queries.
- `.claude/skills/write-script-java/SKILL.md` - MUST use when writing Java scripts.
- `.claude/skills/write-script-mssql/SKILL.md` - MUST use when writing MS SQL Server queries.
- `.claude/skills/write-script-mysql/SKILL.md` - MUST use when writing MySQL queries.
- `.claude/skills/write-script-nativets/SKILL.md` - MUST use when writing Native TypeScript scripts.
- `.claude/skills/write-script-php/SKILL.md` - MUST use when writing PHP scripts.
- `.claude/skills/write-script-postgresql/SKILL.md` - MUST use when writing PostgreSQL queries.
- `.claude/skills/write-script-powershell/SKILL.md` - MUST use when writing PowerShell scripts.
- `.claude/skills/write-script-python3/SKILL.md` - MUST use when writing Python scripts.
- `.claude/skills/write-script-rlang/SKILL.md` - MUST use when writing R scripts.
- `.claude/skills/write-script-rust/SKILL.md` - MUST use when writing Rust scripts.
- `.claude/skills/write-script-snowflake/SKILL.md` - MUST use when writing Snowflake queries.
- `.claude/skills/write-flow/SKILL.md` - MUST use when creating flows.
- `.claude/skills/raw-app/SKILL.md` - MUST use when creating raw apps.
- `.claude/skills/triggers/SKILL.md` - MUST use when configuring triggers.
- `.claude/skills/schedules/SKILL.md` - MUST use when configuring schedules.
- `.claude/skills/resources/SKILL.md` - MUST use when managing resources.
- `.claude/skills/write-workflow-as-code/SKILL.md` - MUST use when writing or modifying Windmill Workflow-as-Code scripts using workflow, task, step, sleep, approvals, taskScript, taskFlow, task_script, or task_flow.
- `.claude/skills/cli-commands/SKILL.md` - MUST use when using the CLI, including debugging job failures and inspecting run history via `wmill job`.
- `.claude/skills/preview/SKILL.md` - MUST use when opening the Windmill dev page / visual preview of a flow, script, or app. Triggers on words like preview, open, navigate to, visualize, see the flow/app/script, and after writing a flow/script/app for visual verification.
