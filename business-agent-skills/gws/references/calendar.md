# Google Calendar（`gws calendar`）

管理日历和事件。

## 快捷命令

### +agenda

显示所有日历的即将发生的事件。

```bash
gws calendar +agenda
```

| Flag | 是否必填 | 默认值 | 说明 |
|------|----------|---------|-------------|
| `--today` | — | — | 显示今日事件 |
| `--tomorrow` | — | — | 显示明日事件 |
| `--week` | — | — | 显示本周事件 |
| `--days` | — | — | 往后多少天 |
| `--calendar` | — | — | 按特定日历名称或 ID 过滤 |
| `--timezone` | — | — | IANA 时区覆盖（如 America/Denver） |

```bash
gws calendar +agenda --today
gws calendar +agenda --week --format table
gws calendar +agenda --days 3 --calendar 'Work'
```

只读。默认查询所有日历。

---

### +insert

创建新事件。

```bash
gws calendar +insert --summary <TEXT> --start <TIME> --end <TIME>
```

| Flag | 是否必填 | 默认值 | 说明 |
|------|----------|---------|-------------|
| `--calendar` | — | primary | 日历 ID |
| `--summary` | 是 | — | 事件标题 |
| `--start` | 是 | — | 开始时间（ISO 8601，如 2026-06-17T09:00:00-07:00） |
| `--end` | 是 | — | 结束时间（ISO 8601） |
| `--location` | — | — | 事件地点 |
| `--description` | — | — | 事件说明 |
| `--attendee` | — | — | 参会人邮箱（可重复） |
| `--meet` | — | — | 添加 Google Meet 链接 |

```bash
gws calendar +insert --summary 'Standup' --start '2026-06-17T09:00:00-07:00' --end '2026-06-17T09:30:00-07:00'
gws calendar +insert --summary 'Review' --start ... --end ... --attendee alice@example.com --meet
```

> **写入命令** —— 若用户已指定日历、时间和参会人，检查目标后执行。邀请他人、周期性事件、批量修改或删除时先确认。

---

## API 资源

- **acl**：`delete`、`get`、`insert`、`list`、`patch`、`update`、`watch`
- **calendarList**：`delete`、`get`、`insert`、`list`、`patch`、`update`、`watch`
- **calendars**：`clear`、`delete`、`get`、`insert`、`patch`、`update`
- **channels**：`stop`
- **colors**：`get`
- **events**：`delete`、`get`、`import`、`insert`、`instances`、`list`、`move`、`patch`、`quickAdd`、`update`、`watch`
- **freebusy**：`query`
- **settings**：`get`、`list`、`watch`

---

## Recipes

### 屏蔽专注时间（Block Focus Time）

1. 创建周期性屏蔽：`gws calendar events insert --params '{"calendarId": "primary"}' --json '{"summary": "Focus Time", "description": "Protected deep work block", "start": {"dateTime": "2025-01-20T09:00:00", "timeZone": "America/New_York"}, "end": {"dateTime": "2025-01-20T11:00:00", "timeZone": "America/New_York"}, "recurrence": ["RRULE:FREQ=WEEKLY;BYDAY=MO,TU,WE,TH,FR"], "transparency": "opaque"}'`
2. 验证：`gws calendar +agenda`

### 查找多日历空闲时间

1. 查询：`gws calendar freebusy query --json '{"timeMin": "2024-03-18T08:00:00Z", "timeMax": "2024-03-18T18:00:00Z", "items": [{"id": "user1@company.com"}, {"id": "user2@company.com"}]}'`
2. 找出重叠的空闲时段
3. 创建事件：`gws calendar +insert --summary 'Meeting' --attendee user1@company.com --attendee user2@company.com --start ... --end ...`

### 改期会议

1. 查找事件：`gws calendar +agenda`
2. 获取详情：`gws calendar events get --params '{"calendarId": "primary", "eventId": "EVENT_ID"}'`
3. 更新：`gws calendar events patch --params '{"calendarId": "primary", "eventId": "EVENT_ID", "sendUpdates": "all"}' --json '{"start": {"dateTime": "2025-01-22T14:00:00", "timeZone": "America/New_York"}, "end": {"dateTime": "2025-01-22T15:00:00", "timeZone": "America/New_York"}}'`

### 安排周期性事件

1. 创建：`gws calendar events insert --params '{"calendarId": "primary"}' --json '{"summary": "Weekly Standup", "start": {"dateTime": "2024-03-18T09:00:00", "timeZone": "America/New_York"}, "end": {"dateTime": "2024-03-18T09:30:00", "timeZone": "America/New_York"}, "recurrence": ["RRULE:FREQ=WEEKLY;BYDAY=MO"], "attendees": [{"email": "team@company.com"}]}'`
2. 验证：`gws calendar +agenda --days 14 --format table`

### 规划一周日程

1. 查看本周：`gws calendar +agenda --week`
2. 查忙闲：`gws calendar freebusy query --json '{"timeMin": "...", "timeMax": "...", "items": [{"id": "primary"}]}'`
3. 添加事件：`gws calendar +insert --summary 'Deep Work Block' --start ... --end ...`

### 向参会人共享会议资料

1. 获取参会人：`gws calendar events get --params '{"calendarId": "primary", "eventId": "EVENT_ID"}'`
2. 逐个共享文件：`gws drive permissions create --params '{"fileId": "FILE_ID"}' --json '{"role": "reader", "type": "user", "emailAddress": "attendee@company.com"}'`
