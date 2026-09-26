# Google Apps Script（`gws script`）

管理 Google Apps Script 项目。

## 快捷命令

### +push

将本地文件上传到 Apps Script 项目。

```bash
gws script +push --script <ID>
```

| Flag | 是否必填 | 默认值 | 说明 |
|------|----------|---------|-------------|
| `--script` | 是 | — | 脚本项目 ID |
| `--dir` | — | — | 存放脚本文件的目录（默认为当前目录） |

```bash
gws script +push --script SCRIPT_ID
gws script +push --script SCRIPT_ID --dir ./src
```

支持 .gs、.js、.html 和 appsscript.json。跳过隐藏文件和 node_modules。会**替换**项目中的所有文件。

> **高风险写入命令** —— 执行前请确认，因为它会替换 Apps Script 项目中的所有文件。

---

## API 资源

- **processes**：`list`、`listScriptProcesses`
- **projects**：`create`、`get`、`getContent`、`getMetrics`、`updateContent`
  - **deployments**：`create`、`delete`、`get`、`list`、`update`
  - **versions**：`create`、`get`、`list`
- **scripts**：`run`
