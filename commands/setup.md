---
description: First-time setup. Configure the LLM API key Antigravity uses to read and analyze your codebase. / 首次 setup，配置 Antigravity 分析代码所需的 LLM API key。
---

You are running first-time setup for the Antigravity plugin. The user just installed the plugin and needs an LLM API key configured before `/antigravity:ag-refresh` and `/antigravity:ag-ask` will work. Goal: write a `.env` file at the current workspace root.

你正在执行 Antigravity 插件的首次 setup。用户刚安装插件，需要先配置 LLM API key，`/antigravity:ag-refresh` 和 `/antigravity:ag-ask` 才能正常工作。目标是在当前工作区根目录写入 `.env` 文件。

## Step 1 — Detect existing config / 步骤 1 —— 检测已有配置

Read `.env` at the workspace root if it exists. If `OPENAI_API_KEY` is already set, ask the user whether to overwrite. If they say no, confirm "already configured" and stop.

如果工作区根目录已有 `.env`，先读取它。如果已经设置了 `OPENAI_API_KEY`，询问用户是否覆盖。若用户选择不覆盖，确认“already configured / 已配置”并停止。

## Step 2 — Ask which LLM provider (use AskUserQuestion) / 步骤 2 —— 询问 LLM 提供商（使用 AskUserQuestion）

Present these options:

向用户展示以下选项：

- **OpenAI** — gpt-4o-mini / gpt-4o
- **DeepSeek** — cheap, strong on code
- **Groq** — fast, free tier
- **阿里灵积 (DashScope)** — qwen 系列
- **NVIDIA NIM** — generous free tier
- **Ollama 本地** — no key needed, runs locally
- **其他 OpenAI 兼容端点** — custom URL

## Step 3 — Collect URL / key / model / 步骤 3 —— 收集 URL / key / model

Use this table to set the URL and suggest a model based on the provider:

根据用户选择的提供商，使用下表设置 URL 并建议模型：

| Provider / 提供商 | `OPENAI_BASE_URL` | Suggested `OPENAI_MODEL` / 建议模型 |
|---|---|---|
| OpenAI | `https://api.openai.com/v1` | `gpt-4o-mini` |
| DeepSeek | `https://api.deepseek.com/v1` | `deepseek-chat` |
| Groq | `https://api.groq.com/openai/v1` | `llama-3.3-70b-versatile` |
| 阿里灵积 | `https://dashscope.aliyuncs.com/compatible-mode/v1` | `qwen-max` |
| NVIDIA NIM | `https://integrate.api.nvidia.com/v1` | `meta/llama-3.3-70b-instruct` |
| Ollama 本地 | `http://localhost:11434/v1` | `llama3.2` (key can be `ollama`) |
| 其他 | ask the user | ask the user |

For non-Ollama providers ask the user to paste their key. For Ollama use `OPENAI_API_KEY=ollama` (the engine requires the field to be non-empty).

非 Ollama 提供商需要让用户粘贴 API key。Ollama 使用 `OPENAI_API_KEY=ollama`（engine 要求该字段非空）。

## Step 4 — Write `.env` / 步骤 4 —— 写入 `.env`

Write to `<workspace>/.env`:

写入 `<workspace>/.env`：

```
OPENAI_BASE_URL=<chosen URL>
OPENAI_API_KEY=<the key, or "ollama">
OPENAI_MODEL=<chosen model>
AG_ASK_TIMEOUT_SECONDS=120
```

If `.env` already existed and the user opted to overwrite, replace only the four keys above; preserve any other lines.

如果 `.env` 已存在且用户选择覆盖，只替换上面四个 key；保留其他行。

## Step 5 — Ensure `.env` is git-ignored / 步骤 5 —— 确保 `.env` 已加入 git ignore

Check `<workspace>/.gitignore`. If `.env` is not listed (and there is no globbing rule that already covers it), append `.env` on a new line. If `.gitignore` doesn't exist, create one with `.env`.

检查 `<workspace>/.gitignore`。如果 `.env` 未列出，且没有其他 glob 规则覆盖它，就追加一行 `.env`。如果 `.gitignore` 不存在，创建一个只包含 `.env` 的文件。

## Step 6 — Tell the user next steps / 步骤 6 —— 告诉用户下一步

Print exactly:

严格输出：

```
✅ Antigravity is configured for this project.
✅ Antigravity 已为当前项目配置完成。

Next / 下一步:
  1. Restart Claude Code (Ctrl+C twice, then re-run claude) so ag-mcp picks up the new .env.
     重启 Claude Code（按两次 Ctrl+C，然后重新运行 claude），让 ag-mcp 读取新的 .env。
  2. /antigravity:ag-refresh        — build the knowledge base (one-time, a few minutes for small repos)
     构建知识库（一次性操作；小仓库通常需要几分钟）。
  3. /antigravity:ag-ask <question> — ask anything about the codebase
     询问任何关于代码库的问题。
```

Do NOT call `mcp__plugin_antigravity_antigravity__refresh_project` from this command — the engine subprocess that's running right now still has the old (empty) env. The user must restart Claude Code first.

不要在本命令中调用 `mcp__plugin_antigravity_antigravity__refresh_project` —— 当前正在运行的 engine 子进程仍然使用旧的（空）env。用户必须先重启 Claude Code。
