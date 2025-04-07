# GET /v1/table

## Description / 説明
テーブル一覧を取得します。
Retrieves a list of available tables.

## Parameters / パラメータ
No parameters required.

## Response / レスポンス
```json
{
    "result": "success",
    "data": [
        {
            "forms": [
                {
                    "label": "ID",
                    "field_id": "id",
                    "type": "number",
                    "required": false,
                    "is_multiple_type": false
                },
                {
                    "label": "文字列(user name lookup)",
                    "field_id": "field__1",
                    "type": "text",
                    "required": false,
                    "is_multiple_type": false
                }
            ],
            "label": "all data type",
            "group": null,
            "table": "dataset__1"
        }
    ]
}
```

## Error Responses / エラーレスポンス

### No Permission / 権限なし
```json
{
    "result": "error",
    "message": "権限がありません"
}
```
