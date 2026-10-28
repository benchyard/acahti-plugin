---
name: acahti
description: >-
  Connect Acahti with OAuth in Codex or personal tokens in Benchyard; use MCP or CLI for repositories, PRs and CI.
  Optional Work/Argos/Chat when whoami.modules flags are on.
  For commits and pushes targeting Acahti, use whoami and setup_local; leave other targets alone.
  Also use for Acahti connection setup and missing MCP tools.
---

# Acahti

## Connect and work

Use native MCP or the `acahti` CLI for the intended instance. Verify `whoami` and read `whoami.modules` before work. On first use, follow the onboarding below even when only the skill was installed.

After connecting, fetch `whoami.skill_url` on `acahti.benchyard.com` for current product workflows. Instance values come from `whoami`. If the live skill is unavailable, report that and use these packaged instructions for supported operations; stop only for operations whose contract cannot be established. Keep the selected transport: Codex/ChatGPT native plugins use OAuth; Benchyard shells use personal Access Tokens.

## Optional modules

Read `whoami.modules` (`work` / `argos` / `chat`). Same flags on `GET /ui/public`. Default all off — do not invent missing tabs.

| Flag | SPA | Notes |
|---|---|---|
| `work` | `$ROOT_URL/tickets`, `/todos`, `/docs` | Top-level tabs (human collaboration; no bygent) |
| `argos` | `$ROOT_URL/argos` | Run browser; ingest `POST $ROOT_URL/api/runs` with `ARGOS_TOKEN`; deep link `/argos/runs/{sid}` |
| `chat` | header launcher + right dock; mobile `/chat` | Org IM |

- Argos CLI: `ARGOS_DASH_URL=$ROOT_URL`. Humans download `dash.env` from Argos → CLI.
- Open a ticket from a failing case only when **both** `work` and `argos` are true.

Follow the live skill **Optional modules** section when `skill_url` is available.

## Instance configuration

This SAIDC workspace release bundles the intended MCP endpoint in `mcp.json`. Reuse that plugin connection. Do not ask members to enter an instance URL or create a duplicate standalone server. Discover its actual runtime identifier; it need not be exactly `acahti`.

The intended instance is `https://acahti.s-aidc.com`. Use it for first-use guidance and `ACAHTI_ROOT_URL`. Each member authorizes the bundled OAuth connection in the client. The package declares OAuth with dynamic client registration; no credentials are shipped. Benchyard shell tasks instead use `ACAHTI_ROOT_URL` and `ACAHTI_TOKEN` in Secrets. Those Secrets do not configure a desktop client. If native MCP is unavailable, continue with the CLI against this instance after verifying `whoami`.

The administrator updates the endpoint through the workspace packaging command. Never guess a different host or transfer credentials when changing an instance.

## First use and connection troubleshooting

1. For desktop plugin setup or repeated Authenticate prompts, read `../setup/SKILL.md`. Discover available MCP tools. If the intended connection works, call `whoami` and continue. In Benchyard or a shell without native MCP tools, use the `acahti` CLI. Check `command -v acahti` and the presence (never values) of `ACAHTI_ROOT_URL` and `ACAHTI_TOKEN`; also try `acahti auth status` for a stored login. Do not read credentials files or extract tokens from Git.
2. If the CLI is missing, install `npm install -g @benchyard/acahti` using the environment's installation procedure. An installed skill does not install the CLI automatically.
3. For a desktop plugin, complete OAuth through the client's connection setup. For Benchyard shell tasks, if configuration is missing, give this concrete guidance in the user's language: “在你的 Acahti 实例打开头像菜单 → Access tokens，创建个人 Token；在 Benchyard 的 Secrets 中设置 ACAHTI_ROOT_URL（实例地址）和 ACAHTI_TOKEN（Token），将 Token 的目标主机限定为该实例。让新变量在当前终端/任务生效后，告诉我继续。” Supply the known instance link. Ask only for missing configuration, never for secret values in chat. Each member uses their own token; workspace packages contain no credentials. Account signup uses an admin invite at `<instance>/join`.
4. Environment credentials are used directly. Benchyard may supply host-bound placeholders: never print, copy, persist, or pass them as literal CLI arguments. For a person outside Benchyard who explicitly wants local storage, `acahti auth login --hostname <instance> --with-token` reads the token on stdin and stores it privately. Bare `acahti auth login` prints setup guidance and exits; it does not open a browser.
5. Verify `acahti whoami` (or native MCP `whoami`), report `login`, `root_url` and enabled `modules`, then run the requested read operation or `acahti inbox`. To verify MCP from a shell, use `acahti mcp tools` and `acahti mcp call whoami`. Successful website login or merely installing the skill is not connection success.

Prefer native MCP when available; the CLI is the supported path in Benchyard. Named CLI commands cover common operations. `acahti mcp tools` discovers the full live catalog; `acahti mcp call TOOL '{"argument":"value"}'` invokes it, and `acahti mcp call TOOL -` accepts a JSON object on stdin for sensitive arguments. Respect the user's authorization for mutations regardless of transport.

For the bundled native HTTP MCP connection, use the client's OAuth flow. A standalone shell client can use an environment-backed Bearer setting. Do not embed it in a shared package or URL. If native MCP is unavailable in a shell task, use the CLI with a personal token. Benchyard Secrets do not populate a desktop client's credentials. Reuse existing server identifiers; do not create duplicate connections. Reload a task only when its tool inventory or environment is stale.

- **401:** check the token's presence, target host and revocation; guide the user to replace it in Secrets. For OAuth, let the client refresh its token; reconnect once if refresh fails. Do not repeatedly retry an invalid token or send a Benchyard user through browser OAuth.
- **403 / permission denied:** authentication may be valid; inspect account/repository permission. Do not request a new login without evidence.
- **Timeout / DNS / TLS / transport error:** report the actual connection failure and endpoint; diagnose reachability separately from credentials.
- **Wrong account or instance:** stop before mutations and resolve the intended configuration. Never send a stored token to another instance.

## Acahti git author

Apply this policy only to commits intended for Acahti and pushes to Acahti. A repository merely having an Acahti remote does not make its other remotes subject to this policy.

1. Resolve the actual repository and intended push destination from the user's request, explicit push remote/URL, or Git's configured push destination (including push URLs). `git commit` itself has no remote; if multiple destinations are plausible, clarify the intended destination before changing identity. Do not infer it from the directory name, remote name, or whichever remote appears first.
2. For an Acahti destination, obtain a fresh result from the matching `whoami` (native MCP or CLI) before each commit-producing operation and compare the destination HTTPS host with `apply_when_remote_host`. Use `git_name` for Author and `git_email` for email directly; `login` identifies the account and is not a replacement for `git_name`. Do not invent or remap names.
3. Before creating a commit for that destination, run `setup_local` in the actual repository using local Git config only. If the identity cannot be verified, stop before creating the commit and diagnose the connection. Then verify `git var GIT_AUTHOR_IDENT` and `git var GIT_COMMITTER_IDENT`. This includes merge/pull operations that create merge commits, amend, rebase, cherry-pick, revert and their continuation commands. Account for `git -c`, `--author` and `GIT_AUTHOR_*` / `GIT_COMMITTER_*` overrides.
4. After creating commits, inspect their actual author and committer. For new original commits (including merge commits and reverts), both must match the verified identity. For amend/rebase/cherry-pick, preserve the expected original author and verify the new committer instead. Before pushing to Acahti, inspect the outgoing commits and verify the author and committer of commits created in this task. A push does not change existing commit metadata. If verified identity is unavailable or a newly created commit has an unexpected identity, stop and resolve it before pushing. Do not relabel imported commits as the current user.
5. Preserve existing authorship when amending, rebasing or cherry-picking another contributor's commit; use the current verified identity for the new committer. Reset an existing author only when the user intends that correction. Never rewrite published history automatically.

For GitHub, Codeup or any other non-Acahti target, do not require Acahti authentication, change identity, block commits, or run island CI. When temporarily applying `setup_local` in a mixed-remote workflow, save and restore the exact prior local identity entries after the Acahti operation so a later non-Acahti commit does not inherit them. Never change global config or delete unrelated identity settings.

Hooks only remind the agent to apply this policy; they do not authenticate, change Git config or enforce a Git-level block.

## Island CI and PRs

When the push or PR target is an Acahti remote (host in `whoami.apply_when_remote_host`):

## Git tags vs image tags

Two namespaces. Do not copy one onto the other.

| | Form | Example |
|---|---|---|
| **git tag** | `v` + semver | `v1.4.0` |
| **OCI / helm `image.tag`** | no `v` | office `dev-{sha}`; HK `1.4.0`; `latest` pointer |

YAML: `${CI_COMMIT_TAG#v}` (the pipe expands it; the runner interpolates `${CI_COMMIT_TAG}`). In `commands:`, write `$IMAGE` not `${IMAGE}`.

Put jobs in `.acahti/pipelines/`. Call official pipes with `pipe: <name>@v1` and `with:`. Do not vendor `.acahti/scripts`, write `uses:`, or call `ACAHTI_RUNNER`. YAML only names island-external secrets (`KUBECONFIG: kubeconfig_office`); never put values in git or `with:`. Acahti packages use the triggering user's identity.

```text
✅ git tag v1.4.0
✅ harbor.example/app:1.4.0
✅ workloads.app.image.tag=${CI_COMMIT_TAG#v}
❌ git tag 1.4.0
❌ harbor.example/app:v1.4.0
❌ image.tag=${CI_COMMIT_TAG}
```

## Branches

Protection is per repository. Call `repo_get` and `branch_list`; do not assume `dev` / `test` / `main` / `release`. Direct-push when `protected` is false. Open a PR when `protected` is true (`pr_create` `base` = `default_branch` unless the user named another). If the remote declines a push, open a PR to the default branch.
- Official release trigger is `git push`, a tag, or opening a PR — not `pipeline_trigger` as a substitute

## After push to the island

1. `git rev-parse HEAD`
2. `checks_wait` with `{owner, name, sha}` — follow `whoami.skill_url` for poll vs snapshot
3. Failed: `pipeline_list` → `pipeline_get` → `pipeline_log` (omit `step` unless needed)
4. Fix and push, or `pipeline_rerun`; `pipeline_cancel` only for a stuck run
5. Green → `pr_merge`; `blocked` → `deploy_approve`
6. Triage: `inbox`

When the user only pushes GitHub/Codeup/`origin` and there is no island push in this turn, do not run island CI tools.

## Island git remotes

Follow `whoami.skill_url` for full detail. This rule covers remotes only.

## Scope

- Island git = HTTPS host in `whoami.apply_when_remote_host`
- Not island: `ssh://`, `:2222`, bare LAN IPs, Codeup, GitHub

## Clone and remote name

- Clone via `whoami.clone_url_template` (owner is `whoami.org`, not a team name)
- Team names are access only — do not clone `team/<repo>`
- Prefer remote name `acahti` for the island URL
- If Codeup/GitHub is already `origin`, keep it; fetch/push the island on `acahti`
- Do not `git fetch --all` just to sync the island

```text
✅ git remote add acahti <whoami.clone_url_template>
✅ git push acahti HEAD:dev
❌ Replace origin with Acahti when another host is the product remote
❌ Paste Bearer tokens or read secrets/ for git auth — use a host-scoped credential helper with the personal token
```

On 401, follow the token troubleshooting above. Never ask for a token in chat.

Island policy in this skill must stay aligned with `cursor/rules/*.mdc` in the same repository.
