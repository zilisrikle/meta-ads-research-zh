# Google Slides（`gws slides`）

读写演示文稿。

## API 资源

- **presentations**：`batchUpdate`、`create`、`get`
  - **pages**：`get`、`getThumbnail`

## 示例

```bash
# 创建演示文稿
gws slides presentations create --json '{"title": "Quarterly Review Q2"}'

# 获取演示文稿元数据
gws slides presentations get --params '{"presentationId": "PRES_ID"}'
```

---

## Recipes

### 创建演示文稿

1. 创建：`gws slides presentations create --json '{"title": "Quarterly Review Q2"}'`
2. 共享：`gws drive permissions create --params '{"fileId": "PRES_ID"}' --json '{"role": "writer", "type": "user", "emailAddress": "team@company.com"}'`
