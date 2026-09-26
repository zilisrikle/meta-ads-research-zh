# Google Forms（`gws forms`）

读写 Google Forms 表单。

## API 资源

- **forms**：`batchUpdate`、`create`、`get`、`setPublishSettings`
  - **responses**：`get`、`list`
  - **watches**：`create`、`delete`、`list`、`renew`

## 示例

```bash
# 创建表单
gws forms forms create --json '{"info": {"title": "Event Feedback", "documentTitle": "Event Feedback Form"}}'

# 列出回复
gws forms forms responses list --params '{"formId": "FORM_ID"}' --format table
```

---

## Recipes

### 创建并分享反馈表单

1. 创建：`gws forms forms create --json '{"info": {"title": "Event Feedback", "documentTitle": "Event Feedback Form"}}'`
2. 从响应中获取表单 URL（responderUri）
3. 发邮件：`gws gmail +send --to attendees@company.com --subject 'Please share your feedback' --body 'Fill out the form: FORM_URL'`

### 收集表单回复

1. 列出表单（ID 未知时）：`gws forms forms list`
2. 获取表单详情：`gws forms forms get --params '{"formId": "FORM_ID"}'`
3. 获取回复：`gws forms forms responses list --params '{"formId": "FORM_ID"}' --format table`
