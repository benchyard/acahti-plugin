# Workspace distribution

## Release archives

Generate each workspace release from the current Codex source instead of editing an older ZIP:

```sh
python3 scripts/package_workspace.py --instance https://your-instance.example --workspace SAIDC --output /path/to/acahti-saidc-1.10.0.zip
```

The instance URL is a release argument, not part of the generic plugin source. The generated package includes the MCP endpoint, current skills, reminder hooks and a release manifest with file hashes. Tests, bytecode and local credentials are excluded. Upload the versioned ZIP and verify the directory version, then test MCP `whoami` (including `modules`) and `inbox` from a fresh member task. If the instance sets `ACAHTI_MODULES=work,argos,chat`, also confirm the Work / Argos / Chat SPA tabs. Hooks must be reviewed/trusted separately after changes; their trust is not shared by the workspace.

## Automatic SAIDC workspace publishing

`workspaces/saidc.json` is the workspace-specific connection and existing plugin ID. The generic `codex/` package stays instance-independent.

Tests: install `tests/requirements.txt`, then run `python3 -m unittest discover -s tests -v`.

On relevant changes to `main`, `.github/workflows/workspace-release.yml` runs tests, generates the workspace package and publishes the `workspace-saidc` branch. The generated version includes the source commit, so changed contents never silently reuse an installed version. `build.json` records provenance. Failed builds do not publish.

Import `https://github.com/benchyard/acahti-plugin` in ChatGPT Admin → Plugins → Add → Import marketplace, leave Path empty and select branch `workspace-saidc`. The marketplace entry uses the distinct name `acahti-saidc` and binds the uploaded **Acahti SAIDC** plugin ID, preserving its workspace installation policy. The legacy **Acahti** plugin stays on its old source with automatic sync paused. A plugin already managed by another GitHub source cannot be adopted by a new source; use a distinct plugin name and its own unmanaged ID during migration. Do not delete the plugin to update it. GitHub-managed plugins are updated by sync rather than ZIP uploads.

After the workflow succeeds, the workspace checks for updates daily; use Marketplaces → Sync now for immediate rollout and check the import report. Changing hooks still requires members to review their trust. Version 1.10.0 uses portable plugin.json and mcp.json with explicit OAuth/DCR auth and a setup skill. Changing endpoint/account requires each member to authorize the intended account in the client connection UI; sync does not share credentials. Benchyard Secrets configure shell tasks only. Confirm the installed version and native MCP whoami in the target desktop client before calling desktop rollout verified.

For rollback, revert the source change on `main` and let the same build/sync process publish a new revision. Do not force-push or manually edit the generated branch. Keep the importing admin's GitHub connection authorized. For another workspace, use its own plugin ID and distribution config; never reuse SAIDC's ID.
