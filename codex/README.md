# Acahti for Codex

Use MCP or CLI for identity, repositories, PRs and CI. Read `whoami.modules` for optional Work / Argos / Chat. The generic package supports any Acahti instance; workspace release archives bundle the chosen endpoint.

## First connection

1. Install the plugin and run **setup**, or ask: **连接 Acahti，检查配置并验证身份和待办**.
2. In Codex/ChatGPT, connect the bundled server and complete OAuth for the intended account. The workspace package explicitly declares OAuth/DCR. No personal-token environment variable is required for this native connection.
3. In Benchyard shell tasks, create a personal token in the instance's avatar menu → Access tokens. Set `ACAHTI_ROOT_URL` and `ACAHTI_TOKEN` in Secrets, limiting the token's destination to that instance. The CLI uses these directly without browser OAuth or localhost callbacks. Never send credentials in chat or copy host-bound placeholders into files.
4. Verify native MCP `whoami` and `inbox` in a fresh task. For CLI-only environments, verify `acahti whoami`, `acahti mcp tools` and `acahti inbox`. Report the transport actually verified.

## Repeated authentication after upgrading

Acahti **0.5.29+** accepts both OAuth access tokens and personal tokens at `/mcp`. Version 0.5.27–0.5.28 rejected OAuth tokens even after a successful login. Upgrade the service, then let the client refresh or reconnect once.

The administrator syncs the `workspace-saidc` marketplace and checks the import report. Verify installed plugin **1.10.0 or newer**, then run setup. This removes version 1.9's mandatory environment-token configuration and declares OAuth explicitly. Do not delete a shared plugin to upgrade it. A GitHub release is not proof that a desktop installed the new package.

For 401, check credential expiry, revocation and instance; for 403, review permissions. Diagnose transport failures separately. See the [official plugin OAuth configuration](https://developers.openai.com/plugins/build/plugins#configure-mcp-authentication).

## Git author

For commits targeting Acahti, follow the skill: fresh MCP or CLI `whoami`, `setup_local`, then verify author and committer. This includes merge/pull-generated commits; inspect outgoing commits before pushing. Preserve original authorship when replaying or amending existing commits. Other destinations keep their identity. Optional `hooks/hooks.json` only reminds the agent; trust it via `/hooks` if you want that. It does not enforce a Git-level block.

## Releases

For SAIDC, merge reviewed changes into `main`. The `workspace-release` GitHub Actions workflow tests this source, bundles `workspaces/saidc.json`, and publishes the generated `workspace-saidc` branch. The workspace marketplace tracks that branch through daily sync or **Sync now**. Do not edit the generated branch or delete the workspace plugin to update it.

Use `scripts/package_workspace.py` for standalone ZIP distributions. The generic plugin remains instance-independent. Workspace sync distributes configuration, not personal tokens; verify the new version in a new task. Changed hooks may require renewed trust in `/hooks`.
