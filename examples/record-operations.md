# Record Operation Examples / レコード操作例

## File Operations / ファイル操作

### Getting Files from Records / レコードからファイル取得
まず、レコードAPIを使ってファイル情報IDを取得します：

```http
GET /v1/record?table=dataset__6
```

レスポンス例:
```json
{
  "result": "success",
  "data": [
    {
      "raw_data": {
        "id": "10",
        "field__1": "サンプルレコード",
        "field__2": "7",  // ここがファイルフィールドのfile_info_id
        "field__3": ["12", "15"]  // 複数ファイルの場合はIDの配列
      },
      "view_data": {
        "id": "10",
        "field__1": "サンプルレコード",
        "field__2": "報告書.pdf",  // 表示用のファイル名
        "field__3": ["画像1.jpg", "画像2.png"]
      }
    }
  ]
}
```

次に、取得したfile_info_idを使ってファイルをダウンロードします：

```http
GET /v1/file?file_info_id=7&table=dataset__6
```

レスポンス: ファイルそのものがダウンロードされます（PDFファイルなど）。

## Get Records with Filtering / フィルター付きレコード取得

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

### Filtered Records / フィルター適用
```http
GET /v1/record?table=dataset__1&limit=10&offset=0&order=field__1 desc&fields[]=field__1&fields[]=field__7
```

### With Related Data / 関連データ付き
```http
GET /v1/record?table=dataset__1&include_relation=true
```

## Create Record with Files / ファイル付きレコード作成

### Single Record Creation / 単一レコード作成
```http
POST /v1/record
Content-Type: application/json

{
    "table": "dataset__1",
    "data": [
        {
            "field__1": "新規ユーザー",
            "field__7": "テキストフィールド",
            "field__8": "<p>HTMLコンテンツ</p>",
            "field__9": "123456"
        }
    ]
}
```

### Multiple Records Creation / 複数レコード作成
```http
POST /v1/record
Content-Type: application/json

{
    "table": "dataset__1",
    "data": [
        {
            "field__1": "ユーザー1",
            "field__7": "テキスト1"
        },
        {
            "field__1": "ユーザー2",
            "field__7": "テキスト2"
        }
    ]
}
```

## Update Multiple Records / 複数レコード更新

### Basic Update / 基本的な更新
```http
PUT /v1/record
Content-Type: application/json

{
    "table": "dataset__1",
    "data": [
        {
            "id": 1,
            "field__1": "更新済みユーザー1",
            "field__7": "更新テキスト1"
        },
        {
            "id": 2,
            "field__1": "更新済みユーザー2",
            "field__7": "更新テキスト2"
        }
    ]
}
```

### Update with Unique IDs / 識別ID使用
```http
PUT /v1/record
Content-Type: application/json

{
    "table_id": "test_dataset_unique_1",
    "data": [
        {
            "id": 1,
            "test_field_unique_1": "更新済みデータ"
        }
    ]
}
```

## Delete Operations / 削除操作

### Delete Specific Records / 特定レコード削除
```http
POST /v1/delete_record
Content-Type: application/json

{
    "table": "dataset__1",
    "id": [1, 2, 3]
}
```

### Delete All Records / 全レコード削除
```http
POST /v1/delete-all-record
Content-Type: application/json

{
    "table": "dataset__1"
}
```

## Common Error Patterns / 一般的なエラーパターン

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
