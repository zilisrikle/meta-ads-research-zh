# Google People / 通讯录（`gws people`）

管理联系人和个人资料。

## API 资源

- **contactGroups**：`batchGet`、`create`、`delete`、`get`、`list`、`update`
  - **members**：`modify`
- **otherContacts**：`copyOtherContactToMyContactsGroup`、`list`、`search`
- **people**：`batchCreateContacts`、`batchUpdateContacts`、`createContact`、`deleteContactPhoto`、`get`、`getBatchGet`、`listDirectoryPeople`、`searchContacts`、`searchDirectoryPeople`、`updateContact`、`updateContactPhoto`
  - **connections**：`list`

## 示例

```bash
# 搜索联系人
gws people people searchContacts --params '{"query": "Alice", "readMask": "names,emailAddresses"}'

# 列出目录中的人员
gws people people listDirectoryPeople --params '{"readMask": "names,emailAddresses", "sources": ["DIRECTORY_SOURCE_TYPE_DOMAIN_PROFILE"], "pageSize": 100}' --format table
```

---

## Recipes

### 将通讯录同步到表格

1. 列出联系人：`gws people people listDirectoryPeople --params '{"readMask": "names,emailAddresses,phoneNumbers", "sources": ["DIRECTORY_SOURCE_TYPE_DOMAIN_PROFILE"], "pageSize": 100}' --format json`
2. 添加表头：`gws sheets +append --spreadsheet SHEET_ID --range Contacts --values 'Name,Email,Phone'`
3. 逐个追加：`gws sheets +append --spreadsheet SHEET_ID --range Contacts --values 'Jane Doe,jane@company.com,+1-555-0100'`
