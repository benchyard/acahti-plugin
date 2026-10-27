# Acahti for Codex

Use MCP or CLI for identity, repositories, PRs and CI. Read `whoami.modules` for optional Work / Argos / Chat. The generic package supports any Acahti instance; workspace release archives bundle the chosen endpoint.

## First connection

1. Install the plugin and ask: **连接 Acahti，检查配置并验证身份和待办**. The first invocation starts guided setup; installation itself cannot execute a skill.
2. Create a personal token in your instance's avatar menu → Access tokens. Set `ACAHTI_ROOT_URL` and `ACAHTI_TOKEN` in Benchyard Secrets, limiting the token's destination to that instance. Never send the token in chat.
3. The agent checks existing MCP or CLI access, installs `@benchyard/acahti` if needed, and verifies `whoami` and `inbox`. Environment changes may require a new task. Benchyard placeholders remain in the environment and are never copied to files.
4. Workspace packages preconfigure the endpoint and `bearer_token_env_var: ACAHTI_TOKEN`. Generic native Codex: `codex mcp add <name> --url <endpoint> --bearer-token-env-var ACAHTI_TOKEN`. Reuse existing connections. For shell tasks, `acahti mcp tools` and `acahti mcp call` expose the full catalog without native MCP tools.

401 means check token/host/revocation; permission errors require access review; network errors require connection diagnosis. No OAuth or localhost callback is used.

## Git author

For commits targeting Acahti, follow the skill: fresh MCP or CLI `whoami`, `setup_local`, then verify author and committer. This includes merge/pull-generated commits; inspect outgoing commits before pushing. Preserve original authorship when replaying or amending existing commits. Other destinations keep their identity. Optional `hooks/hooks.json` only reminds the agent; trust it via `/hooks` if you want that. It does not enforce a Git-level block.

## Releases

For SAIDC, merge reviewed changes into `main`. The `workspace-release` GitHub Actions workflow tests this source, bundles `workspaces/saidc.json`, and publishes the generated `workspace-saidc` branch. The workspace marketplace tracks that branch through daily sync or **Sync now**. Do not edit the generated branch or delete the workspace plugin to update it.

Use `scripts/package_workspace.py` for standalone ZIP distributions. The generic plugin remains instance-independent. Workspace sync distributes configuration, not personal tokens; verify the new version in a new task. Changed hooks may require renewed trust in `/hooks`.
