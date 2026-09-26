# 跨服务工作流（`gws workflow`）

结合多个 Workspace 产品、面向常见任务的内置命令。

## +standup-report

今日会议 + 未完成任务，生成站会摘要。

```bash
gws workflow +standup-report
gws workflow +standup-report --format table
```

结合今日日历议程与任务列表。只读。

---

## +meeting-prep

为下一场会议做准备：议程、参会人、关联文档。

```bash
gws workflow +meeting-prep
gws workflow +meeting-prep --calendar Work
```

| Flag | 默认值 | 说明 |
|------|---------|-------------|
| `--calendar` | primary | 日历 ID |
| `--format` | json | 输出格式：json、table、yaml、csv |

显示下一场即将到来的事件及其参会人和说明。只读。

---

## +email-to-task

将一封 Gmail 邮件转为 Google Tasks 条目。

```bash
gws workflow +email-to-task --message-id <ID>
```

| Flag | 是否必填 | 默认值 | 说明 |
|------|----------|---------|-------------|
| `--message-id` | 是 | — | Gmail 邮件 ID |
| `--tasklist` | — | @default | 任务列表 ID |

读取邮件主题作为任务标题，摘要作为备注。

> **写入命令** —— 若用户已指定邮件/任务目标，检查目标后执行。收件人、目标列表或副作用含糊时先确认。

---

## +weekly-digest

每周摘要：本周会议 + 未读邮件数。

```bash
gws workflow +weekly-digest
gws workflow +weekly-digest --format table
```

结合本周日历议程与 gmail 整理摘要。只读。

---

## +file-announce

在 Chat 空间中公布一个 Drive 文件。

```bash
gws workflow +file-announce --file-id <ID> --space <SPACE>
```

| Flag | 是否必填 | 说明 |
|------|---------|-------------|
| `--file-id` | 是 | Drive 文件 ID |
| `--space` | 是 | Chat 空间名（如 spaces/ABC123） |
| `--message` | — | 自定义公布消息 |
| `--format` | json | 输出格式：json、table、yaml、csv |

从 Drive 获取文件名以构建公布消息。

> **写入命令** —— 若用户已指定文件和 Chat 空间，检查目标后执行。面向外部/广泛发布或权限变更时先确认。
