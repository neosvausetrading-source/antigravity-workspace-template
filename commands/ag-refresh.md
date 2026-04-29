---
description: Rebuild the antigravity project knowledge base after significant changes. / 在重要改动后重建 antigravity 项目知识库。
allowed-tools: ["mcp__plugin_antigravity_antigravity__refresh_project"]
---

Call `mcp__plugin_antigravity_antigravity__refresh_project` for the current workspace.

调用 `mcp__plugin_antigravity_antigravity__refresh_project`，为当前工作区刷新项目知识库。

If $ARGUMENTS contains "quick" → pass `quick=true`. Otherwise pass `quick=false`.

如果 $ARGUMENTS 包含 "quick"，传入 `quick=true`；否则传入 `quick=false`。

If the MCP tool is not available or not connected, do not diagnose this as an LLM key problem. Tell the user to restart Claude Code once so the Antigravity MCP server loads, then rerun `/antigravity:ag-refresh`. Point them to `docs/en/TROUBLESHOOTING.md` for the diagnostic log path.

如果 MCP 工具不可用或未连接，不要判断为 LLM API key 问题。请告诉用户完整重启 Claude Code 一次，让 Antigravity MCP server 加载完成，然后重新运行 `/antigravity:ag-refresh`。诊断日志路径见 `docs/en/TROUBLESHOOTING.md`。

Report progress concisely; full refresh can take several minutes.

简洁汇报进度；完整 refresh 可能需要几分钟。
