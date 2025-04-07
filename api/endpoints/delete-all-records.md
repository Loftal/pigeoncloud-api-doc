# POST /v1/delete-all-record

## Description / 説明
テーブルの全レコードを削除します。
Deletes all records from a table.

## Request Body / リクエストボディ
```json
{
    "table": "dataset__1"
}
```

### Parameters / パラメータ
- `table` (必須/required): データセットテーブル名 / Dataset table name
  - Format: `dataset__[id]` or table unique ID
  - Example: `dataset__1`, `test_dataset_unique_1`

## Response / レスポンス
```json
{
    "result": "success",
    "data": {
        "job_id": 1
    }
}
```

### Job Status Check / ジョブステータス確認
削除は非同期で実行されます。ステータスは以下のエンドポイントで確認できます：
Deletion is performed asynchronously. Check status with:

```
GET /v1/job-status/{job_id}
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
