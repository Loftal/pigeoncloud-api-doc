# GET /v1/fields

## Description / 説明
テーブルの項目定義を取得します。
Retrieves field definitions for a table.

## Parameters / パラメータ
### Required / 必須
- `table`: データセットテーブル名 / Dataset table name
  - Format: `dataset__[id]` or table unique ID
  - Example: `dataset__1`, `test_dataset_unique_1`

## Response / レスポンス
```json
{
    "result": "success",
    "data": [
        {
            "label": "文字列",
            "field_id": "field__1",
            "type": "text",
            "required": false,
            "is_multiple_type": false
        },
        {
            "label": "数値",
            "field_id": "field__2",
            "type": "number",
            "required": false,
            "is_multiple_type": false
        }
    ]
}
```

## Error Responses / エラーレスポンス

### Missing Table / テーブル未指定
```json
{
    "result": "error",
    "message": "tableは必須です"
}
```

### Invalid Table / 無効なテーブル
```json
{
    "result": "error",
    "message": "tableが見つかりません"
}
```

### No Permission / 権限なし
```json
{
    "result": "error",
    "message": "権限がありません"
}
```
