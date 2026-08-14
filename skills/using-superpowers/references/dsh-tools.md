# DeepSeek Harness Tool Reference

This is the DSH platform adaptation for the superpowers skills. Read this file
when a skill mentions a tool, hook, or mechanism that does not exist on this
harness — the equivalent lives here.

## How skills are invoked here

The DeepSeek Harness exposes a `skill` tool. When a skill applies, call it
BEFORE acting:

- `skill(name: "brainstorming")` loads the skill body into context with the
  same `<skill_content>` framing the catalog advertises.
- The catalog lists every available skill with its `whenToUse` guidance.
  Superpowers skills are discoverable under their bare names (`brainstorming`,
  `writing-plans`, `systematic-debugging`, ...).

There is no `CLAUDE.md` auto-loading on DSH. Put durable project instructions
in `AGENTS.md` (or the session's system prompt), and load skills explicitly.

## Core tool mapping (Claude Code → DSH)

| Claude Code | DSH equivalent | Notes |
| --- | --- | --- |
| `Bash` | `pwsh` (Windows) / `bash` (POSIX) | PowerShell on Windows hosts; bash on POSIX hosts. |
| `Read` / `Write` / `Edit` | `read` / `write` / `edit` | Same semantics; DSH adds `sandbox_permissions` escalation on write/edit. |
| `Glob` / `Grep` | `glob` / `grep` | `glob` returns files only (never directories); `grep` is ripgrep syntax. |
| `TodoWrite` | `todo_write` | Whole-list replacement each call. |
| `Task` (subagents) | `subagent` / `subagent_fork` | Background by default; `subagent_fork` inherits this conversation. |
| `AskUserQuestion` | `ask_user_question` | Questions return stable ids echoed in answers. |
| `WebSearch` | `web_search` | Returns a summary answer plus source URLs. |
| `LS` (file list) | `glob` + `read` | There is no dedicated `ls` tool. |
| Hooks (PreToolUse etc.) | None | DSH has no hook system. Enforce workflows through skills themselves. |
| `/commands` slash commands | `dsh` CLI + `command-*` plugins | Slash-command UI lives in the web surface. |
| Plan mode | `exit_plan_mode` | Present the plan; on approval leave plan mode and execute. |
| `ReadImage` | `read_image` | PNG/JPEG/WebP/GIF only. |
| Background jobs | `run_in_background: true` on tools | `job_output` / `job_kill` / `job_list` manage them. |

## DSH-specific tools superpowers should know about

- `goal` tools (`create_goal`, `get_goal`, `update_goal`): persisted
  same-session completion objectives with automatic continuation rounds.
- `workflow`: fan work out across many subagents with phases and structured
  results — the DSH-native way to run `dispatching-parallel-agents` at scale.
- `ralph`: fresh-agent iterative loops (only when the human explicitly asks).
- `skill`: loads a skill's full instructions (see above).

## Windows notes

- `pwsh` runs in ConstrainedLanguage under the read-only sandbox; commands
  that need .NET APIs may require `workspace-write` or `danger-full-access`.
- `bash`-only scripts (the `start-server.sh` helpers in brainstorming) do
  not run on Windows; use `pwsh` equivalents or the `node` scripts instead.
- Paths use `C:\...` form; read env vars with `$env:NAME` in pwsh.
