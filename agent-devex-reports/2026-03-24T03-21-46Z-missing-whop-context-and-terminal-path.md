# DevEx Report: missing whop context and terminal path
## Task
Update the onboarding docs so they tell people to install Tailscale before using Third Eye.

## DevEx issues encountered
- Workspace instructions required using Whop MCP tools before any other action, but no MCP resources/tools were available in this environment.
- The provided terminals folder path in task context did not exist on disk, so the initial required check for existing terminal sessions failed immediately.
- The requested file name was `onboarding.mdx`, but this repo did not contain that page yet; discovering the correct docs entry point required extra repo exploration.

## Impact
- The missing Whop tooling created ambiguity about whether the required workflow could actually be followed, and added extra investigation steps before work could begin.
- The invalid terminals path caused a failed command and slowed the initial setup/check phase.
- The mismatch between the requested file name and the repo structure increased search time and token usage before implementation.

## Reproduction
- Attempt to list MCP resources at task start and observe that no Whop resources are available.
- Attempt to inspect the provided terminals folder path: `/home/ubuntu/.cursor/projects/workspace/terminals`.
- Search the repo for `onboarding.mdx` and onboarding references; note that only `quickstart.mdx` existed before the change.

## Proposed improvements
- Ensure Whop MCP tools/resources are attached in environments where workspace rules require them.
- Validate and inject the correct terminals directory path into task context before agent startup.
- Include the canonical target path for requested docs edits when creating tasks, especially in docs repos with starter-template content.

## Notes
- Repo: `/workspace`
- Missing terminals path error: `ls: cannot access '/home/ubuntu/.cursor/projects/workspace/terminals': No such file or directory`
- The task was completed by adding `/workspace/onboarding.mdx` and wiring it into navigation.
