# GET /v1/file

## Description / 説明
ファイルをダウンロードします。
Downloads a file.

## Parameters / パラメータ
### Required / 必須
- `file_info_id`: ファイル情報ID / File information ID
  - Type: integer
  - Example: `file_info_id=123`

- `table`: データセットテーブル名 / Dataset table name
  - Format: `dataset__[id]` or table unique ID
  - Example: `dataset__1`, `test_dataset_unique_1`

## Response / レスポンス
File content with appropriate Content-Type header.

## Error Responses / エラーレスポンス

### Missing File ID / ファイルID未指定
```json
{
    "result": "error",
    "message": "file_info_idは必須です"
}
```

### Invalid File / 無効なファイル
```json
{
    "result": "error",
    "message": "info error"
}
```
