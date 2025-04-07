# POST /v1/delete_record

## Description / 説明
レコードを削除します。
Deletes records.

## Request Body / リクエストボディ
```json
{
    "table": "dataset__1",
    "id": [1, 2, 3]
}
```

### Parameters / パラメータ
- `table` (必須/required): データセットテーブル名 / Dataset table name
  - Format: `dataset__[id]` or table unique ID
  - Example: `dataset__1`, `test_dataset_unique_1`

- `id` (必須/required): 削除するレコードIDの配列 / Array of record IDs to delete
  - Type: array of integers

## Response / レスポンス
```json
{
    "result": "success",
    "data": {
        "deleted": [1, 2, 3]
    }
}
```

## Error Responses / エラーレスポンス

### Invalid Table / 無効なテーブル
```json
{
    "result": "error",
    "message": "tableは必須です"
}
```

### No Permission / 権限なし
```json
{
    "result": "error",
    "message": "権限がありません"
}
```

### Invalid IDs / 無効なID
```json
{
    "result": "error",
    "message": "削除対象のレコードが見つかりません"
}
```
