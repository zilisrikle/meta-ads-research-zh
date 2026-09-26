# Google Drive（`gws drive`）

管理文件、文件夹和共享云端硬盘。

## 快捷命令

### +upload

上传文件（自动生成元数据）。

```bash
gws drive +upload <file>
```

| Flag | 是否必填 | 默认值 | 说明 |
|------|----------|---------|-------------|
| `<file>` | 是 | — | 文件路径 |
| `--parent` | — | — | 父文件夹 ID |
| `--name` | — | — | 目标文件名（默认为源文件名） |

```bash
gws drive +upload ./report.pdf
gws drive +upload ./report.pdf --parent FOLDER_ID
gws drive +upload ./data.csv --name 'Sales Data.csv'
```

MIME 类型自动检测。

> **写入命令** —— 若用户已指定本地文件和目标位置，检查目标后执行。修改共享/权限、删除、移动或批量操作时先确认。

---

## API 资源

- **about**：`get`
- **accessproposals**：`get`、`list`、`resolve`
- **apps**：`get`、`list`
- **changes**：`getStartPageToken`、`list`、`watch`
- **channels**：`stop`
- **comments**：`create`、`delete`、`get`、`list`、`update`
- **drives**：`create`、`get`、`hide`、`list`、`unhide`、`update`
- **files**：`copy`、`create`、`download`、`export`、`generateIds`、`get`、`list`、`listLabels`、`modifyLabels`、`update`、`watch`
- **operations**：`get`
- **permissions**：`create`、`delete`、`get`、`list`、`update`
- **replies**：`create`、`delete`、`get`、`list`、`update`
- **revisions**：`delete`、`get`、`list`、`update`

---

## Recipes

### 批量下载文件夹

1. 列出文件：`gws drive files list --params '{"q": "'\''FOLDER_ID'\'' in parents"}' --format json`
2. 逐个下载：`gws drive files get --params '{"fileId": "FILE_ID", "alt": "media"}' -o filename.ext`
3. 将 Google Docs 导出为 PDF：`gws drive files export --params '{"fileId": "FILE_ID", "mimeType": "application/pdf"}' -o document.pdf`

### 创建共享云端硬盘

1. 创建：`gws drive drives create --params '{"requestId": "unique-id"}' --json '{"name": "Project X"}'`
2. 添加成员：`gws drive permissions create --params '{"fileId": "DRIVE_ID", "supportsAllDrives": true}' --json '{"role": "writer", "type": "user", "emailAddress": "member@company.com"}'`

### 整理 Drive 文件夹

1. 创建文件夹：`gws drive files create --json '{"name": "Q2 Project", "mimeType": "application/vnd.google-apps.folder"}'`
2. 创建子文件夹：`gws drive files create --json '{"name": "Documents", "mimeType": "application/vnd.google-apps.folder", "parents": ["PARENT_ID"]}'`
3. 移动文件：`gws drive files update --params '{"fileId": "FILE_ID", "addParents": "FOLDER_ID", "removeParents": "OLD_PARENT"}'`
4. 验证：`gws drive files list --params '{"q": "FOLDER_ID in parents"}' --format table`

### 查找大文件

1. 按大小列出：`gws drive files list --params '{"orderBy": "quotaBytesUsed desc", "pageSize": 20, "fields": "files(id,name,size,mimeType,owners)"}' --format table`

### 向团队共享文件夹

1. 查找文件夹：`gws drive files list --params '{"q": "name = '\''Project X'\'' and mimeType = '\''application/vnd.google-apps.folder'\''"}'`
2. 以编辑者共享：`gws drive permissions create --params '{"fileId": "FOLDER_ID"}' --json '{"role": "writer", "type": "user", "emailAddress": "colleague@company.com"}'`
3. 以查看者共享：`gws drive permissions create --params '{"fileId": "FOLDER_ID"}' --json '{"role": "reader", "type": "user", "emailAddress": "stakeholder@company.com"}'`
4. 验证：`gws drive permissions list --params '{"fileId": "FOLDER_ID"}' --format table`

### 通过邮件发送 Drive 文件链接

1. 查找文件：`gws drive files list --params '{"q": "name = '\''Quarterly Report'\''"}'`
2. 共享：`gws drive permissions create --params '{"fileId": "FILE_ID"}' --json '{"role": "reader", "type": "user", "emailAddress": "client@example.com"}'`
3. 发邮件：`gws gmail +send --to client@example.com --subject 'Quarterly Report' --body 'Report link: https://docs.google.com/document/d/FILE_ID'`
