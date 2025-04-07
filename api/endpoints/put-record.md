# PUT /v1/record

## Description / 説明
既存のレコードを更新します。
Updates existing records.

## Request Body / リクエストボディ
```json
{
    "table": "dataset__1",
    "data": [
        {
            "id": 1,
            "field__1": "更新されたユーザー",
            "field__7": "更新テキスト",
            "field__8": "<p>更新HTML</p>",
            "field__9": "456789"
        }
    ]
}
```

### Parameters / パラメータ
- `table` (必須/required): データセットテーブル名 / Dataset table name
  - Format: `dataset__[id]` or table unique ID
  - Example: `dataset__1`, `test_dataset_unique_1`

- `data` (必須/required): レコードデータの配列 / Array of record data
  - Type: array of objects
  - Each object must contain:
    - `id`: Record ID to update
    - Field values to update

## Response / レスポンス
```json
{
    "result": "success",
    "data": [
        {
            "id": 1,
            "error": null
        }
    ]
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

### Record Not Found / レコード未検出
```json
{
    "result": "error",
    "message": "レコードが見つかりません"
}
```
