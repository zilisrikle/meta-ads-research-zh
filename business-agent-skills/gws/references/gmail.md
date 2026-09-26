# Gmail（`gws gmail`）

发送、读取和管理邮件。

## 目录

- [快捷命令（Helper Commands）](#helper-commands)
  - [+send](#send)
  - [+read](#read)
  - [+reply](#reply)
  - [+reply-all](#reply-all)
  - [+forward](#forward)
  - [+triage](#triage)
  - [+watch](#watch)
- [API 资源（API Resources）](#api-resources)

---

## 快捷命令

### +send

发送邮件。

```bash
gws gmail +send --to <EMAILS> --subject <SUBJECT> --body <TEXT>
```

| Flag | 是否必填 | 默认值 | 说明 |
|------|----------|---------|-------------|
| `--to` | 是 | — | 收件人邮箱，逗号分隔 |
| `--subject` | 是 | — | 邮件主题 |
| `--body` | 是 | — | 邮件正文（纯文本；加 --html 则为 HTML） |
| `--from` | — | — | 发件人地址（send-as 别名） |
| `--cc` | — | — | 抄送邮箱，逗号分隔 |
| `--bcc` | — | — | 密送邮箱，逗号分隔 |
| `--attach` | — | — | 文件附件（可重复，总上限 25MB） |
| `--html` | — | — | 将 --body 视为 HTML（使用片段标签，无需包裹结构） |
| `--draft` | — | — | 保存为草稿而不是发送 |
| `--dry-run` | — | — | 预览而不执行 |

```bash
gws gmail +send --to alice@example.com --subject 'Hello' --body 'Hi Alice!'
gws gmail +send --to alice@example.com --subject 'Report' --body 'See attached' -a report.pdf
gws gmail +send --to alice@example.com --subject 'Hello' --body '<b>Bold</b>' --html
gws gmail +send --to alice@example.com --subject 'Hello' --body 'Hi!' --draft
```

> **写入命令** —— 预览用 `--draft` 或 `--dry-run`。发送外部邮件前请确认，除非用户已明确给出收件人、主题和正文并要求发送。

---

### +read

读取一封邮件并提取正文或邮件头。

```bash
gws gmail +read --id <ID>
```

| Flag | 是否必填 | 默认值 | 说明 |
|------|----------|---------|-------------|
| `--id` | 是 | — | Gmail 邮件 ID |
| `--headers` | — | — | 包含邮件头（From、To、Subject、Date） |
| `--format` | — | text | 输出格式（text、json） |
| `--html` | — | — | 返回 HTML 正文而非纯文本 |

```bash
gws gmail +read --id 18f1a2b3c4d
gws gmail +read --id 18f1a2b3c4d --headers
gws gmail +read --id 18f1a2b3c4d --format json | jq '.body'
```

自动将纯 HTML 邮件转换为纯文本，并处理 multipart / base64 解码。

---

### +reply

回复邮件（自动处理线程关联）。

```bash
gws gmail +reply --message-id <ID> --body <TEXT>
```

| Flag | 是否必填 | 默认值 | 说明 |
|------|----------|---------|-------------|
| `--message-id` | 是 | — | 要回复的 Gmail 邮件 ID |
| `--body` | 是 | — | 回复正文 |
| `--from` | — | — | 发件人地址（send-as 别名） |
| `--to` | — | — | 额外收件人 |
| `--cc` | — | — | 抄送收件人 |
| `--bcc` | — | — | 密送收件人 |
| `--attach` | — | — | 文件附件（可重复） |
| `--html` | — | — | HTML 模式 |
| `--draft` | — | — | 保存为草稿 |

自动设置 In-Reply-To、References 和 threadId，并引用原文。

> **写入命令** —— 若用户已指定要回复的邮件和回复正文，检查目标收件人后执行。回复多个或外部收件人时先确认。

---

### +reply-all

全部回复（自动处理线程关联）。

```bash
gws gmail +reply-all --message-id <ID> --body <TEXT>
```

标志与 `+reply` 相同，另加：

| Flag | 说明 |
|------|-------------|
| `--remove` | 从回复中排除的收件人（逗号分隔的邮箱） |

回复发件人和所有原始 To/CC 收件人。若排除后没有剩余 To 收件人则失败。

> **写入命令** —— 若用户已指定要回复的邮件和回复正文，检查收件人集合后执行。回复众多或外部收件人时先确认。

---

### +forward

将邮件转发给新收件人。

```bash
gws gmail +forward --message-id <ID> --to <EMAILS>
```

| Flag | 是否必填 | 默认值 | 说明 |
|------|----------|---------|-------------|
| `--message-id` | 是 | — | 要转发的 Gmail 邮件 ID |
| `--to` | 是 | — | 收件人邮箱 |
| `--from` | — | — | 发件人地址（send-as 别名） |
| `--body` | — | — | 转发邮件上方附带的说明 |
| `--no-original-attachments` | — | — | 排除原附件 |
| `--attach` | — | — | 额外文件附件 |
| `--cc` | — | — | 抄送收件人 |
| `--bcc` | — | — | 密送收件人 |
| `--html` | — | — | HTML 模式 |
| `--draft` | — | — | 保存为草稿 |

```bash
gws gmail +forward --message-id 18f1a2b3c4d --to dave@example.com
gws gmail +forward --message-id 18f1a2b3c4d --to dave@example.com --body 'FYI see below'
```

> **写入命令** —— 若用户已指定要转发的邮件和转发收件人，检查目标后执行。转发给外部收件人或涉及敏感内容时先确认。

---

### +triage

显示未读收件箱摘要（发件人、主题、日期）。

```bash
gws gmail +triage
```

| Flag | 是否必填 | 默认值 | 说明 |
|------|----------|---------|-------------|
| `--max` | — | 20 | 最多显示的邮件数 |
| `--query` | — | is:unread | Gmail 搜索查询 |
| `--labels` | — | — | 包含标签名 |

```bash
gws gmail +triage
gws gmail +triage --max 5 --query 'from:boss'
gws gmail +triage --labels --format table
```

只读。默认表格输出。

---

### +watch

监听新邮件并以 NDJSON 流式输出。

```bash
gws gmail +watch
```

| Flag | 是否必填 | 默认值 | 说明 |
|------|----------|---------|-------------|
| `--project` | — | — | Pub/Sub 的 GCP 项目 ID |
| `--subscription` | — | — | 已有的 Pub/Sub 订阅（跳过创建） |
| `--topic` | — | — | 已有的 Pub/Sub 主题 |
| `--label-ids` | — | — | 按标签 ID 过滤（如 INBOX,UNREAD） |
| `--max-messages` | — | 10 | 每次拉取的最大邮件数 |
| `--poll-interval` | — | 5 | 拉取间隔秒数 |
| `--msg-format` | — | full | 邮件格式：full、metadata、minimal、raw |
| `--once` | — | — | 拉取一次后退出 |
| `--cleanup` | — | — | 退出时删除 Pub/Sub 资源 |
| `--output-dir` | — | — | 将每封邮件写入单独的 JSON 文件 |

Gmail watch 7 天后过期 —— 重新运行以续期。Ctrl-C 停止。

---

## API 资源

### users

- `getProfile`、`stop`、`watch`
- **drafts**：`create`、`delete`、`get`、`list`、`send`、`update`
- **history**：`list`
- **labels**：`create`、`delete`、`get`、`list`、`patch`、`update`
- **messages**：`batchDelete`、`batchModify`、`delete`、`get`、`import`、`insert`、`list`、`modify`、`send`、`trash`、`untrash`
  - **attachments**：`get`
- **settings**：`getAutoForwarding`、`getImap`、`getLanguage`、`getPop`、`getVacation`、`updateAutoForwarding`、`updateImap`、`updateLanguage`、`updatePop`、`updateVacation`
  - **cse**：identity / keypair 管理
  - **delegates**：`create`、`delete`、`get`、`list`
  - **filters**：`create`、`delete`、`get`、`list`
  - **forwardingAddresses**：`create`、`delete`、`get`、`list`
  - **sendAs**：`create`、`delete`、`get`、`list`、`patch`、`update`、`verify`
    - **smimeInfo**：`delete`、`get`、`insert`、`list`、`setDefault`
- **threads**：`delete`、`get`、`list`、`modify`、`trash`、`untrash`

---

## Recipes

### 创建 Gmail 过滤器

自动为收到的邮件打标签、加星或分类。

1. 列出现有标签：`gws gmail users labels list --params '{"userId": "me"}' --format table`
2. 创建新标签：`gws gmail users labels create --params '{"userId": "me"}' --json '{"name": "Receipts"}'`
3. 创建过滤器：`gws gmail users settings filters create --params '{"userId": "me"}' --json '{"criteria": {"from": "receipts@example.com"}, "action": {"addLabelIds": ["LABEL_ID"], "removeLabelIds": ["INBOX"]}}'`
4. 验证：`gws gmail users settings filters list --params '{"userId": "me"}' --format table`

### 设置外出自动回复

1. 启用：`gws gmail users settings updateVacation --params '{"userId": "me"}' --json '{"enableAutoReply": true, "responseSubject": "Out of Office", "responseBodyPlainText": "I am out of the office until Jan 20. For urgent matters, contact backup@company.com.", "restrictToContacts": false, "restrictToDomain": false}'`
2. 验证：`gws gmail users settings getVacation --params '{"userId": "me"}'`
3. 回来后关闭：`gws gmail users settings updateVacation --params '{"userId": "me"}' --json '{"enableAutoReply": false}'`

### 为邮件打标签并归档

1. 搜索：`gws gmail users messages list --params '{"userId": "me", "q": "from:notifications@service.com"}' --format table`
2. 打标签：`gws gmail users messages modify --params '{"userId": "me", "id": "MSG_ID"}' --json '{"addLabelIds": ["LABEL_ID"]}'`
3. 归档：`gws gmail users messages modify --params '{"userId": "me", "id": "MSG_ID"}' --json '{"removeLabelIds": ["INBOX"]}'`

### 转发带标签的邮件

1. 查找：`gws gmail users messages list --params '{"userId": "me", "q": "label:needs-review"}' --format table`
2. 读取：`gws gmail +read --id MSG_ID`
3. 转发：`gws gmail +forward --message-id MSG_ID --to manager@company.com`

### 将邮件附件保存到 Drive

1. 搜索：`gws gmail users messages list --params '{"userId": "me", "q": "has:attachment from:client@example.com"}' --format table`
2. 获取邮件：`gws gmail users messages get --params '{"userId": "me", "id": "MSG_ID"}'`
3. 下载附件：`gws gmail users messages attachments get --params '{"userId": "me", "messageId": "MSG_ID", "id": "ATTACHMENT_ID"}'`
4. 上传：`gws drive +upload ./attachment.pdf --parent FOLDER_ID`

### 将邮件保存为文档

1. 查找邮件：`gws gmail +triage --query 'subject:important from:boss@company.com'`
2. 读取：`gws gmail +read --id MSG_ID`
3. 创建文档：`gws docs documents create --json '{"title": "Saved Email - Important Update"}'`
4. 写入正文：`gws docs +write --document DOC_ID --text '...'`
