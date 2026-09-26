# Google Sheets（`gws sheets`）

读写电子表格。

## 快捷命令

### +read

读取电子表格中的数值。

```bash
gws sheets +read --spreadsheet <ID> --range <RANGE>
```

| Flag | 是否必填 | 默认值 | 说明 |
|------|----------|---------|-------------|
| `--spreadsheet` | 是 | — | 电子表格 ID |
| `--range` | 是 | — | A1 记法表示的范围（如 `Sheet1!A1:D10`） |

```bash
gws sheets +read --spreadsheet ID --range "Sheet1!A1:D10"
gws sheets +read --spreadsheet ID --range Sheet1
```

只读。

---

### +append

向电子表格追加一行。

```bash
gws sheets +append --spreadsheet <ID>
```

| Flag | 是否必填 | 默认值 | 说明 |
|------|----------|---------|-------------|
| `--spreadsheet` | 是 | — | 电子表格 ID |
| `--values` | — | — | 逗号分隔的值（简单字符串） |
| `--json-values` | — | — | 行的 JSON 数组，如 `'[["a","b"],["c","d"]]'` |
| `--range` | — | A1 | 目标范围（用于选择特定工作表） |

```bash
gws sheets +append --spreadsheet ID --values 'Alice,100,true'
gws sheets +append --spreadsheet ID --json-values '[["a","b"],["c","d"]]'
gws sheets +append --spreadsheet ID --range "Sheet2!A1" --values 'Alice,100'
```

> **写入命令** —— 若用户已指定电子表格/范围和数值，检查目标后执行。批量更新、清空、覆盖或范围含糊时先确认。

---

## API 资源

- **spreadsheets**：`batchUpdate`、`create`、`get`、`getByDataFilter`
  - **developerMetadata**：`get`、`search`
  - **sheets**：`copyTo`
  - **values**：`append`、`batchClear`、`batchGet`、`batchGetByDataFilter`、`batchUpdate`、`batchUpdateByDataFilter`、`clear`、`get`、`update`

---

## Recipes

### 将工作表备份为 CSV

1. 读取数值：`gws sheets +read --spreadsheet SHEET_ID --range Sheet1 --format csv`
2. 或通过 Drive 导出：`gws drive files export --params '{"fileId": "SHEET_ID", "mimeType": "text/csv"}'`

### 对比工作表标签页

1. 读取标签页 1：`gws sheets +read --spreadsheet SHEET_ID --range "January!A1:D"`
2. 读取标签页 2：`gws sheets +read --spreadsheet SHEET_ID --range "February!A1:D"`
3. 对比数据

### 为新月份复制工作表

1. 获取详情：`gws sheets spreadsheets get --params '{"spreadsheetId": "SHEET_ID"}'`
2. 复制标签页：`gws sheets spreadsheets sheets copyTo --params '{"spreadsheetId": "SHEET_ID", "sheetId": 0}' --json '{"destinationSpreadsheetId": "SHEET_ID"}'`
3. 重命名：`gws sheets spreadsheets batchUpdate --params '{"spreadsheetId": "SHEET_ID"}' --json '{"requests": [{"updateSheetProperties": {"properties": {"sheetId": 123, "title": "February 2025"}, "fields": "title"}}]}'`

### 创建费用跟踪表

1. 创建电子表格：`gws drive files create --json '{"name": "Expense Tracker 2025", "mimeType": "application/vnd.google-apps.spreadsheet"}'`
2. 添加表头：`gws sheets +append --spreadsheet SHEET_ID --values 'Date,Category,Description,Amount'`
3. 添加条目：`gws sheets +append --spreadsheet SHEET_ID --values '2025-01-15,Travel,Flight to NYC,450.00'`

### 记录商机更新

1. 查找表格：`gws drive files list --params '{"q": "name = '\''Sales Pipeline'\'' and mimeType = '\''application/vnd.google-apps.spreadsheet'\''"}'`
2. 读取：`gws sheets +read --spreadsheet SHEET_ID --range "Pipeline!A1:F"`
3. 追加：`gws sheets +append --spreadsheet SHEET_ID --range Pipeline --values '2024-03-15,Acme Corp,Proposal Sent,$50000,Q2,jdoe'`

### 从表格创建事件

1. 读取数据：`gws sheets +read --spreadsheet SHEET_ID --range "Events!A2:D"`
2. 逐行创建：`gws calendar +insert --summary '...' --start ... --end ... --attendee ...`

### 从表格数据生成报告

1. 读取数据：`gws sheets +read --spreadsheet SHEET_ID --range "Sales!A1:D"`
2. 创建文档：`gws docs documents create --json '{"title": "Sales Report - January 2025"}'`
3. 写报告：`gws docs +write --document DOC_ID --text '...'`
4. 共享：`gws drive permissions create --params '{"fileId": "DOC_ID"}' --json '{"role": "reader", "type": "user", "emailAddress": "cfo@company.com"}'`
