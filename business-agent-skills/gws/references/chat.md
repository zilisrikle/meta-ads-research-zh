# Google Chat（`gws chat`）

管理 Chat 空间、成员、消息和媒体。

## 快捷命令

### +send

向空间发送纯文本消息。

```bash
gws chat +send --space <NAME> --text <TEXT>
```

| Flag | 是否必填 | 默认值 | 说明 |
|------|----------|---------|-------------|
| `--space` | 是 | — | 空间名，如 `spaces/AAAA...` |
| `--text` | 是 | — | 消息文本 |

```bash
gws chat +send --space spaces/AAAAxxxx --text 'Hello team!'
```

用 `gws chat spaces list` 查找空间名。卡片、线程回复、附件或更新请使用下面的原生 API 资源。

> **写入命令** —— 若用户已指定空间和消息，检查目标后执行。面向外部/广泛发布、权限变更或收件人含糊时先确认。

---

## API 资源

- **customEmojis**：`create`、`delete`、`get`、`list`
- **media**：`download`、`upload`
- **spaces**：`completeImport`、`create`、`delete`、`findDirectMessage`、`findGroupChats`、`get`、`list`、`patch`、`search`、`setup`
  - **members**：用 `gws chat spaces members --help` 查看
  - **messages**：`create`、`delete`、`get`、`list`、`patch`、`update`
    - **attachments**：用 `gws chat spaces messages attachments --help` 查看
    - **reactions**：用 `gws chat spaces messages reactions --help` 查看
  - **spaceEvents**：用 `gws chat spaces spaceEvents --help` 查看
- **users**：`spaces`、`sections`

---

## 示例

```bash
# 列出调用者可见的空间
gws chat spaces list --format table

# 获取一个空间
gws chat spaces get --params '{"name": "spaces/AAAAxxxx"}'

# 列出空间中的消息
gws chat spaces messages list --params '{"parent": "spaces/AAAAxxxx"}' --format table

# 通过原生 API 创建一条基本消息
gws chat spaces messages create --params '{"parent": "spaces/AAAAxxxx"}' --json '{"text": "Hello team!"}'
```

---

## Recipes

### 公布文件

1. 确保收件人能访问该文件：`gws drive permissions create --params '{"fileId": "FILE_ID"}' --json '{"role": "reader", "type": "user", "emailAddress": "team@company.com"}'`
2. 发送链接：`gws chat +send --space spaces/AAAAxxxx --text 'New file: https://drive.google.com/file/d/FILE_ID/view'`

### 查找私聊空间

1. 搜索：`gws chat spaces findDirectMessage --params '{"name": "users/person@example.com"}'`
2. 发送：`gws chat +send --space spaces/DM_SPACE --text 'Quick update...'`

### 回顾空间近期消息

1. 列出空间：`gws chat spaces list --format table`
2. 列出消息：`gws chat spaces messages list --params '{"parent": "spaces/AAAAxxxx", "pageSize": 20}' --format table`
3. 获取单条消息：`gws chat spaces messages get --params '{"name": "spaces/AAAAxxxx/messages/MESSAGE_ID"}'`
