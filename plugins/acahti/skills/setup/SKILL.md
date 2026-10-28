---
name: setup
description: Set up the Acahti plugin after installation, verify the intended account, or diagnose missing tools and repeated authentication prompts.
---

# Connect Acahti

Read `../acahti/SKILL.md` for the instance and connection procedure. Workspace packages include the instance; generic packages ask for it only if no existing connection or CLI configuration identifies it.

1. Discover the intended native MCP connection and call `whoami` if available. A successful website login, authentication callback, or installed package alone does not prove that tools work.
2. In Codex/ChatGPT, use the bundled OAuth connection. Complete the client's authorization flow for the intended instance and account. Do not require a desktop environment variable or ask for credentials in chat. Reuse the connection rather than creating another server.
3. If authorization succeeds but tools remain unavailable, inspect the loaded plugin version and source, MCP endpoint, and actual error. The server must accept OAuth tokens (Acahti 0.5.29+ restores this alongside personal tokens). Plugin 1.10.0+ declares OAuth explicitly. A workspace administrator syncs the marketplace; a GitHub release alone does not update installed copies. Reopen the conversation only after a changed package or connection has loaded. Do not delete shared plugins or repeatedly reauthorize.
4. In Benchyard shell tasks, check only the presence of `ACAHTI_ROOT_URL` and `ACAHTI_TOKEN` in Secrets. Follow the Acahti skill's CLI setup without browser OAuth or localhost callbacks. Never print or copy host-bound placeholders into files or desktop settings. Benchyard Secrets and desktop OAuth credentials are separate.
5. Verify `whoami` and `inbox` through the working transport. Report the account, instance and enabled modules. State whether native plugin tools were verified or only CLI access. When Work is enabled, also discover ticket_get. Missing Work tools on an otherwise authenticated connection require a server capability upgrade, not another login. Resume the user's task when its required connection works.

For OAuth 401, allow client refresh; reconnect once if refresh fails. For personal-token 401, check the intended host and revocation, then replace the token in Secrets. For 403, inspect permissions. Diagnose network and client capability failures separately from authentication. Missing tools alone do not prove an authentication failure.
