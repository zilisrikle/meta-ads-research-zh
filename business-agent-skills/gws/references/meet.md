# Google Meet（`gws meet`）

管理 Google Meet 会议。

## API 资源

- **conferenceRecords**：`get`、`list`
  - **participants**：`get`、`list`
    - **participantSessions**：`get`、`list`
  - **recordings**：`get`、`list`
  - **smartNotes**：`get`、`list`
  - **transcripts**：`get`、`list`
    - **entries**：`get`、`list`
- **spaces**：`create`、`endActiveConference`、`get`、`patch`

## 示例

```bash
# 创建会议空间
gws meet spaces create --json '{"config": {"accessType": "OPEN"}}'

# 列出最近的会议
gws meet conferenceRecords list --format table

# 列出参会者
gws meet conferenceRecords participants list --params '{"parent": "conferenceRecords/CONF_ID"}' --format table
```

---

## Recipes

### 创建 Meet 空间并分享链接

1. 创建：`gws meet spaces create --json '{"config": {"accessType": "OPEN"}}'`
2. 从响应中获取 URI
3. 发邮件：`gws gmail +send --to team@company.com --subject 'Join the meeting' --body 'Join here: MEETING_URI'`

### 回顾 Meet 出席情况

1. 列出会议：`gws meet conferenceRecords list --format table`
2. 列出参会者：`gws meet conferenceRecords participants list --params '{"parent": "conferenceRecords/CONF_ID"}' --format table`
3. 获取会话详情：`gws meet conferenceRecords participants participantSessions list --params '{"parent": "conferenceRecords/CONF_ID/participants/PARTICIPANT_ID"}' --format table`
