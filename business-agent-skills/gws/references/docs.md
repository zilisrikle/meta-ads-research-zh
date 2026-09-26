# Google Docs（`gws docs`）

读写 Google Docs 文档。

## 快捷命令

### +write

向文档追加文本。

```bash
gws docs +write --document <ID> --text <TEXT>
```

| Flag | 是否必填 | 默认值 | 说明 |
|------|----------|---------|-------------|
| `--document` | 是 | — | 文档 ID |
| `--text` | 是 | — | 要追加的文本（纯文本） |

```bash
gws docs +write --document DOC_ID --text 'Hello, world!'
```

文本插入到文档正文末尾。需要富格式时请使用原生 `batchUpdate` API。

> **写入命令** —— 若用户已指定文档和文本，检查目标后执行。大型编辑、模板覆盖或共享/权限变更时先确认。

---

## API 资源

- **documents**：`batchUpdate`、`create`、`get`

---

## Recipes

### 从模板创建文档

1. 复制模板：`gws drive files copy --params '{"fileId": "TEMPLATE_DOC_ID"}' --json '{"name": "Project Brief - Q2 Launch"}'`
2. 添加内容：`gws docs +write --document NEW_DOC_ID --text '## Project: Q2 Launch ...'`
3. 共享：`gws drive permissions create --params '{"fileId": "NEW_DOC_ID"}' --json '{"role": "writer", "type": "user", "emailAddress": "team@company.com"}'`

### 从文档起草邮件

1. 获取文档：`gws docs documents get --params '{"documentId": "DOC_ID"}'`
2. 发送：`gws gmail +send --to recipient@example.com --subject 'Newsletter Update' --body 'CONTENT_FROM_DOC'`

### 共享文档并通知

1. 共享：`gws drive permissions create --params '{"fileId": "DOC_ID"}' --json '{"role": "writer", "type": "user", "emailAddress": "reviewer@company.com"}'`
2. 发邮件：`gws gmail +send --to reviewer@company.com --subject 'Please review: Project Brief' --body 'Link: https://docs.google.com/document/d/DOC_ID'`
