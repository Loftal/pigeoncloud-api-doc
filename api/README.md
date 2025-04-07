# PigeonCloud API Documentation

## Overview / 概要
PigeonCloudのAPIドキュメント。このAPIを使用することで、PigeonCloudのデータの取得、作成、更新、削除が可能です。

The PigeonCloud API documentation. This API allows you to retrieve, create, update, and delete data in PigeonCloud.

## Authentication / 認証
APIキーを使用した認証が必要です:
Authentication is required using an API key:

- Header: `X-Pigeon-Authorization`
- Format: API key

各リクエストのヘッダーにAPIキーを含める必要があります。
The API key must be included in the header of each request.

## Rate Limiting / レート制限
APIリクエスト制限があります:
API request limits are in place:

- リクエストの頻度が高すぎる場合、429ステータスコードが返されます
- 429 status code will be returned if request frequency is too high

## Common Parameters / 共通パラメータ
### Query Parameters
- `table` (必須/required): データセットテーブル名 / Dataset table name
  - Format: `dataset__[id]` or table unique ID
- `limit` (任意/optional): 1ページあたりの結果数 / Results per page
  - Default: 20
  - Maximum: 100
- `offset` (任意/optional): ページネーションオフセット / Pagination offset
  - Default: 0

### Response Format / レスポンス形式
```json
{
    "result": "success",
    "data": [...],
    "count": 0,
    "offset": 0,
    "limit": 20
}
```

## Error Responses / エラーレスポンス
```json
{
    "result": "error",
    "message": "エラーメッセージ / Error message"
}
```

Common status codes / 主なステータスコード:
- 200: Success / 成功
- 400: Bad Request / リクエスト不正
- 401: Unauthorized / 認証エラー
- 403: Forbidden / アクセス権限なし
- 429: Too Many Requests / リクエスト制限超過
