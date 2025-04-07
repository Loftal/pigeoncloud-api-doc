# GET /v1/record

## Description / 説明
レコード一覧を取得します。
Retrieves a list of records.

## Parameters / パラメータ
### Required / 必須
- `table`: データセットテーブル名 / Dataset table name
  - Format: `dataset__[id]` or table unique ID
  - Example: `dataset__1`, `test_dataset_unique_1`

### Optional / 任意
- `limit`: 1ページあたりの結果数 / Results per page
  - Default: 20
  - Maximum: 100
  - Example: `limit=50`

- `offset`: ページネーションオフセット / Pagination offset
  - Default: 0
  - Example: `offset=20`

- `order`: ソート順 / Sort order
  - Format: `{field} {direction}`
  - Direction: `asc` or `desc`
  - Example: `order=field__1 desc`

- `filter_id`: フィルターID / Filter ID
  - Example: `filter_id=1`

- `fields`: 取得するフィールド / Fields to retrieve
  - Format: Array of field IDs
  - Example: `fields[]=field__1&fields[]=field__2`

- `include_relation`: 関連データを含める / Include related data
  - Values: `true` or `false`
  - Default: `false`

## Examples / 使用例

### Basic Record Retrieval / 基本的なレコード取得
```http
GET /v1/record?table=dataset__1
```

Response:
```json
{
    "result": "success",
    "data": [
        {
            "raw_data": {
                "id": "2",
                "field__1": "マスターユーザー",
                "field__7": "あああ あああ",
                "field__8": "<p>あああ</p><p><strong>あああ</strong></p><p>あああ</p>",
                "field__9": "123321"
            },
            "view_data": {
                "id": "2",
                "field__1": "マスターユーザー",
                "field__7": "あああ あああ",
                "field__8": "<p>あああ</p><p><strong>あああ</strong></p><p>あああ</p>",
                "field__9": "123,321"
            },
            "permission": {
                "view": true,
                "edit": true,
                "delete": true
            }
        }
    ],
    "count": 1,
    "offset": 0,
    "limit": 20
}
```

### Filtered and Sorted Records / フィルター・ソート付きレコード取得
```http
GET /v1/record?table=dataset__1&limit=10&offset=0&order=field__1 desc&fields[]=field__1&fields[]=field__7
```

### Records with Related Data / 関連データ付きレコード取得
```http
GET /v1/record?table=dataset__1&include_relation=true
```

### Using Table Unique ID / テーブル識別IDを使用
```http
GET /v1/record?table_id=test_dataset_unique_1
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

### Invalid Limit / 無効な制限値
```json
{
    "result": "error",
    "message": "limitは0から100の範囲で指定して下さい"
}
```
