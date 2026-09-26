# Google Keep（`gws keep`）

在企业环境中管理 Google Keep 笔记。

## API 资源

- **media**：`download`
- **notes**：`create`、`delete`、`get`、`list`
  - **permissions**：用 `gws keep notes permissions --help` 查看

---

## 示例

```bash
# 列出笔记
gws keep notes list --format table

# 获取一条笔记
gws keep notes get --params '{"name": "notes/NOTE_ID"}'

# 创建笔记
gws keep notes create --json '{"title": "Meeting notes", "body": {"text": {"text": "Discuss launch timeline."}}}'
```

---

## 说明

- Keep API 旨在供企业环境使用。
- 删除笔记要求调用者拥有 `OWNER` 角色。
- 删除笔记立即生效且不可撤销；协作者将失去访问权限。
- 结果较多时请使用 `notes list` 响应中的分页字段。

> **写入/删除命令** —— 若用户已指定笔记内容，检查目标后可运行 `create`。`delete` 或权限变更前先确认。

---

## Recipes

### 记录会议笔记

1. 创建笔记：`gws keep notes create --json '{"title": "Meeting: Project X", "body": {"text": {"text": "Decisions:\n- ...\nActions:\n- ..."}}}'`
2. 验证：`gws keep notes list --format table`

### 导出笔记以供审阅

1. 列出笔记：`gws keep notes list --format json`
2. 获取一条笔记：`gws keep notes get --params '{"name": "notes/NOTE_ID"}'`
3. 如需在别处写总结：`gws docs +write --document DOC_ID --text '...'`
