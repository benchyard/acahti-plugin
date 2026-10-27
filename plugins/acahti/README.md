# Acahti — SAIDC workspace

Version: `1.9.0+saidc.gitecd4b8931661`

MCP endpoint: `https://acahti.s-aidc.com/mcp`

## 首次使用

1. 安装插件后发送：连接 Acahti 并检查我的身份和待办。Skill 首次调用会检查连接与配置。
2. 在实例头像菜单 → Access tokens 创建个人 Token，在 Benchyard Secrets 设置 ACAHTI_ROOT_URL 和 ACAHTI_TOKEN；Token 目标主机限定为该实例。
3. 让环境变量在当前任务生效。Agent 使用原生 MCP 或 acahti CLI，无需 OAuth 或 localhost 回调。
4. 成功调用 whoami 和 inbox 后才算连接成功；MCP 可通过 acahti mcp tools 验证。不要把 Token 发到聊天中。

## Author 提醒

可在 /hooks 审查并信任提醒 hook，需要 Python 3。它只注入规则，不读取凭据、不改 Git、不强制拦截提交。Skill 要求 Acahti 提交前核验当前 MCP 身份、按 remote host 限定范围、设置并检查有效 author。

## 发布维护

从最新通用源码用 scripts/package_workspace.py 重新生成包，不在旧 ZIP 上累积修改。release.json 记录源码文件哈希和包内文件哈希，用来核对发布内容。每次修改必须使用新版本。
