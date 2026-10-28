# Acahti — SAIDC workspace

Version: `1.10.0+saidc.git51193212a965`

MCP endpoint: `https://acahti.s-aidc.com/mcp`

## 首次使用

1. 安装插件后发送：连接 Acahti 并检查我的身份和待办。Skill 首次调用会检查连接与配置。
2. Codex/ChatGPT 点击连接并完成 OAuth 授权。Benchyard 终端任务使用个人 Access Token，在 Secrets 设置 ACAHTI_ROOT_URL 和 ACAHTI_TOKEN，限定目标主机。
3. 运行插件 setup 并验证 whoami。若授权成功但工具不可用，核对服务端版本、插件同步和实际工具加载；不要反复授权。Benchyard 新变量需在当前任务生效。
4. 成功调用 whoami 和 inbox 后才算连接成功；MCP 可通过 acahti mcp tools 验证。不要把 Token 发到聊天中。

## Author 提醒

可在 /hooks 审查并信任提醒 hook，需要 Python 3。它只注入规则，不读取凭据、不改 Git、不强制拦截提交。Skill 要求 Acahti 提交前核验当前 MCP 身份、按 remote host 限定范围、设置并检查有效 author。

## 发布维护

从最新通用源码用 scripts/package_workspace.py 重新生成包，不在旧 ZIP 上累积修改。release.json 记录源码文件哈希和包内文件哈希，用来核对发布内容。每次修改必须使用新版本。
