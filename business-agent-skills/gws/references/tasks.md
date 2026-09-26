# Google Tasks（`gws tasks`）

管理任务列表和任务。

## API 资源

- **tasklists**：`delete`、`get`、`insert`、`list`、`patch`、`update`
- **tasks**：`clear`、`delete`、`get`、`insert`、`list`、`move`、`patch`、`update`

---

## 示例

```bash
# 列出任务列表
gws tasks tasklists list --format table

# 创建任务列表
gws tasks tasklists insert --json '{"title": "Project X"}'

# 列出任务列表中的任务
gws tasks tasks list --params '{"tasklist": "TASKLIST_ID"}' --format table

# 创建任务
gws tasks tasks insert --params '{"tasklist": "TASKLIST_ID"}' --json '{"title": "Follow up with Alice", "notes": "Send the revised brief."}'
```

---

## 说明

- 默认不返回从 Docs 或 Chat 空间指派的任务。
- 从 Docs 或 Chat 空间指派的任务无法通过 Tasks API 插入；需从指派界面创建。
- 在 Tasks 中删除已指派的任务可能会同时删除 Docs 或 Chat 空间中的已指派任务和原始任务。
- `tasks clear` 会隐藏所选列表中已完成的任务，而非永久删除所有已完成任务。

> **写入/删除命令** —— 若用户已指定列表和任务内容，检查目标后可运行 `insert`。`clear`、`delete`、批量移动或含糊更新前先确认。

---

## Recipes

### 创建项目任务列表

1. 创建列表：`gws tasks tasklists insert --json '{"title": "Project X"}'`
2. 添加第一个任务：`gws tasks tasks insert --params '{"tasklist": "TASKLIST_ID"}' --json '{"title": "Draft plan", "notes": "Owner: Alice"}'`
3. 验证：`gws tasks tasks list --params '{"tasklist": "TASKLIST_ID"}' --format table`

### 将会议跟进事项转为任务

1. 读取会议详情：`gws calendar events get --params '{"calendarId": "primary", "eventId": "EVENT_ID"}'`
2. 创建任务：`gws tasks tasks insert --params '{"tasklist": "TASKLIST_ID"}' --json '{"title": "Follow up: ACTION_ITEM"}'`

### 清理已完成任务

1. 查看列表：`gws tasks tasks list --params '{"tasklist": "TASKLIST_ID", "showCompleted": true}' --format table`
2. 清空已完成：`gws tasks tasks clear --params '{"tasklist": "TASKLIST_ID"}'`
