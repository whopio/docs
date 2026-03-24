# DevEx Report: docs cleanup tooling friction
## Task
Clean up low-substance internal documentation pages by removing empty or placeholder content, updating `docs.json`, and pruning empty folders.

## DevEx issues encountered
- The Whop MCP workflow is marked as mandatory in workspace rules, but no MCP resources were available in this environment, so the required first step could not actually be performed.
- The terminal metadata path provided in the task context did not exist on disk, which prevented the expected pre-check for running terminal sessions.
- `python` was not installed, so a quick repository analysis command failed and had to be retried with `python3`.

## Impact
- Missing MCP availability created uncertainty about whether required context tooling was misconfigured or simply unavailable, adding verification work before starting the actual cleanup.
- The missing terminal metadata path caused an avoidable failed command and interrupted the normal process for checking existing sessions.
- The absent `python` binary caused a failed analysis command and a small amount of rework during content auditing.

## Reproduction
- Attempt to access MCP context at task start using the available MCP resource tooling in this workspace; no resources are returned.
- Run `ls "/home/ubuntu/.cursor/projects/workspace/terminals"` in this environment; the path does not exist.
- Run `python - <<'PY' ... PY` in this repo; the shell returns `python: command not found`, while `python3` works.

## Proposed improvements
- Expose the Whop MCP server consistently in environments where the workflow is mandatory, or gate the rule when the server is unavailable.
- Ensure the task context provides a valid terminal metadata path, or omit the path when the feature is unavailable.
- Provide a stable `python` shim to `python3`, or document that only `python3` is guaranteed in the environment.

## Notes
- Missing terminal path from task context: `/home/ubuntu/.cursor/projects/workspace/terminals`
- Successful fallback interpreter: `python3`
- Repo path used for task: `/workspace`
