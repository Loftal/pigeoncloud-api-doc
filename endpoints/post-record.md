# POST /v1/record

## Description / 説明
新しいレコードを作成します。
Creates new records.

## Request Body / リクエストボディ
```json
{
    "table": "dataset__1",
    "data": [
        {
            "field__1": "マスターユーザー",
            "field__7": "あああ あああ",
            "field__8": "<p>あああ</p><p><strong>あああ</strong></p><p>あああ</p>",
            "field__9": "123321"
        }
    ],
    "notify": true
}
```

### Parameters / パラメータ
- `table` (必須/required): データセットテーブル名 / Dataset table name
  - Format: `dataset__[id]` or table unique ID
  - Example: `dataset__1`, `test_dataset_unique_1`

- `data` (必須/required): レコードデータの配列 / Array of record data
  - Type: array of objects
  - Each object contains field values

- `notify` (任意/optional): 通知を送信するか / Whether to send notifications
  - Type: boolean
  - Default: true

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

### Validation Error / バリデーションエラー
```json
{
    "result": "error",
    "message": "validation error",
    "data": [
        {
            "id": null,
            "error": "パラメータエラー"
        }
    ]
}
```
