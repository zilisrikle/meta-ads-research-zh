---
name: gws
description: "在受支持的 Google Workspace 服务上执行 `gws` CLI 操作：Gmail、Calendar、Drive、Docs、Sheets、Slides、Forms、Meet、Contacts、Chat、Tasks、Keep 和 Apps Script。涵盖发送邮件、日历事件、Drive 文件、电子表格、文档、演示文稿、表单、Meet 录制、联系人、Chat 消息、任务、Keep 笔记以及 Apps Script 项目。"
---

# Google Workspace CLI（`gws`）

统一的 Google Workspace API CLI。各产品通过 `gws <service> <command>` 访问。

## 环境准备

### 安装

`gws` 二进制文件必须位于 `$PATH` 中。安装选项见[项目仓库](https://github.com/googleworkspace/cli)。

### 认证

```bash
# 基于浏览器的 OAuth —— 使用 -s 仅选择需要的服务
gws auth login -s gmail,calendar,drive,sheets,docs,forms,slides,chat,tasks,keep

# 当任务不需要写入权限时使用只读 OAuth
gws auth login --readonly

# 已有的凭证文件或令牌
export GOOGLE_WORKSPACE_CLI_CREDENTIALS_FILE=/path/to/credentials.json
export GOOGLE_WORKSPACE_CLI_TOKEN=ya29...
```

凭证优先级：先是 `GOOGLE_WORKSPACE_CLI_TOKEN`，然后是 `GOOGLE_WORKSPACE_CLI_CREDENTIALS_FILE`，再然后是存储在 gws 配置目录中的浏览器 OAuth 凭证。对于浏览器 OAuth，需提供 `GOOGLE_WORKSPACE_CLI_CLIENT_ID` 和 `GOOGLE_WORKSPACE_CLI_CLIENT_SECRET`，或在 `~/.config/gws` 中配置。使用 `gws auth status` 查看当前认证状态（不打印密钥）。

## CLI 语法

```bash
gws <service> <resource> [sub-resource] <method> [flags]
```

快捷命令使用 `+` 前缀表示常用操作：

```bash
gws gmail +send --to alice@example.com --subject 'Hello' --body 'Hi!'
gws calendar +agenda --today
gws sheets +read --spreadsheet ID --range "Sheet1!A1:D10"
```

### 全局标志（Global Flags）

| Flag | 说明 |
|------|-------------|
| `--format <FORMAT>` | 输出格式：`json`（默认）、`table`、`yaml`、`csv` |
| `--dry-run` | 仅本地校验，不调用 API |
| `--sanitize <TEMPLATE>` | 通过 Model Armor 对响应做内容筛查 |

### 方法标志（Method Flags）

| Flag | 说明 |
|------|-------------|
| `--params '{"key": "val"}'` | URL / 查询参数 |
| `--json '{"key": "val"}'` | 请求体 |
| `-o, --output <PATH>` | 将二进制响应保存到文件 |
| `--upload <PATH>` | 上传文件内容（multipart） |
| `--page-all` | 自动分页（NDJSON 输出） |
| `--page-limit <N>` | 使用 --page-all 时的最大页数（默认：10） |
| `--page-delay <MS>` | 分页请求之间的延迟（毫秒，默认：100） |

### 发现命令（Discovering Commands）

在调用任何 API 方法之前，先检查它：

```bash
gws <service> --help                          # 浏览资源和方法
gws schema <service>.<resource>.<method>      # 检查参数、类型、默认值
```

## 安全规则

- **绝不**直接输出密钥（API keys、tokens）
- 只读命令可直接运行
- 若用户已明确要求执行特定低风险写入（创建草稿、追加一行、创建笔记/任务/文档/事件），检查命令中的目标 ID / 收件人后执行
- 高风险写入前需确认：发送外部邮件或 Chat 消息、修改权限/共享、删除/清空数据、批量更新、公开发布，或影响多个用户/文件/事件的操作
- 对于破坏性、批量或含糊的操作，优先使用 `--dry-run` 或简洁的命令预览
- 对 PII / 内容安全筛查使用 `--sanitize`

## Shell 技巧

- **zsh `!` 展开：** Sheet 范围如 `Sheet1!A1` 含有 `!`，会被 zsh 解释为历史展开。请使用双引号：
  ```bash
  gws sheets +read --spreadsheet ID --range "Sheet1!A1:D10"
  ```
- **带双引号的 JSON：** 用单引号包裹 `--params` 和 `--json` 的值：
  ```bash
  gws drive files list --params '{"pageSize": 5}'
  ```

## 可用产品

需要详细的 API 资源、快捷命令标志或分步操作指南时，请阅读相应的参考文件。

| 产品 | Service | Helpers | 参考文件 |
|---------|---------|---------|-----------|
| **Gmail** | `gws gmail` | `+send` `+read` `+reply` `+reply-all` `+forward` `+triage` `+watch` | [references/gmail.md](references/gmail.md) |
| **Calendar** | `gws calendar` | `+agenda` `+insert` | [references/calendar.md](references/calendar.md) |
| **Drive** | `gws drive` | `+upload` | [references/drive.md](references/drive.md) |
| **Docs** | `gws docs` | `+write` | [references/docs.md](references/docs.md) |
| **Sheets** | `gws sheets` | `+read` `+append` | [references/sheets.md](references/sheets.md) |
| **Slides** | `gws slides` | — | [references/slides.md](references/slides.md) |
| **Forms** | `gws forms` | — | [references/forms.md](references/forms.md) |
| **Meet** | `gws meet` | — | [references/meet.md](references/meet.md) |
| **People** | `gws people` | — | [references/people.md](references/people.md) |
| **Chat** | `gws chat` | `+send` | [references/chat.md](references/chat.md) |
| **Tasks** | `gws tasks` | — | [references/tasks.md](references/tasks.md) |
| **Keep** | `gws keep` | — | [references/keep.md](references/keep.md) |
| **Apps Script** | `gws script` | `+push` | [references/script.md](references/script.md) |

### 跨服务工作流（Cross-Service Workflows）

针对常见任务的内置多产品命令：

| 命令 | 说明 |
|---------|-------------|
| `gws workflow +standup-report` | 今日会议 + 未完成任务 |
| `gws workflow +meeting-prep` | 下一场会议：议程、参会人、关联文档 |
| `gws workflow +email-to-task` | 将一封 Gmail 邮件转为 Tasks 条目 |
| `gws workflow +weekly-digest` | 本周会议 + 未读邮件数 |
| `gws workflow +file-announce` | 在 Chat 空间中公布一个 Drive 文件 |

详情见 [references/workflows.md](references/workflows.md)。

每个产品参考文件还包含常见多步任务的分步 **Recipes**（如 Gmail 过滤器、批量日历事件、Drive 整理、报告生成等）。
