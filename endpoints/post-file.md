# POST /v1/record (ファイルアップロード / File Upload)

## Description / 説明
新しいレコードを作成し、ファイルをアップロードします。
Creates new records with file uploads.

## Single File Upload / 単一ファイルアップロード

### Request / リクエスト
```
POST /v1/record
Content-Type: multipart/form-data
```

### Parameters / パラメータ
- `table` (必須/required): データセットテーブル名 / Dataset table name
  - Format: `dataset__[id]` or table unique ID
  - Example: `dataset__1`, `test_dataset_unique_1`

- `data[0][field_name]` (任意/optional): フィールド値 / Field value
  - Example: `data[0][field__39]`

- `field_name` (必須/required): アップロードするファイルフィールド / File field to upload
  - Type: file
  - Example: `field__39`

### Example (Python) / 例（Python）
```python
def singleImgPost(self, table, field_name, file_path, value):
    fileDataBinary = open(file_path, 'rb').read()
    files = {field_name: (file_path, fileDataBinary, 'image/png')}
    data = {
        'table': table,
        f'data[0][{field_name}]': value
    }
    response = self.post(self.getApiUrl('record'), data=data, headers=self.getHeader(), files=files)
    return response
```

## Multiple Files Upload / 複数ファイルアップロード

### Request / リクエスト
```
POST /v1/record
Content-Type: multipart/form-data
```

### Parameters / パラメータ
- `table` (必須/required): データセットテーブル名 / Dataset table name
  - Format: `dataset__[id]` or table unique ID
  - Example: `dataset__1`, `test_dataset_unique_1`

- `field_name[]` (必須/required): アップロードする複数ファイルフィールド / Multiple files field to upload
  - Type: file array
  - Example: `field__39[]`

### Example (Python) / 例（Python）
```python
def multiImgPost(self, table, field_name, file_paths):
    files = {}
    for i, file_path in enumerate(file_paths):
        fileDataBinary = open(file_path, 'rb').read()
        files[f'{field_name}[]'] = (file_path, fileDataBinary, 'image/png')

    data = {'table': table}

    response = self.post(self.getApiUrl('record'), data=data, headers=self.getHeader(), files=files)
    return response
```

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

### File Upload Error / ファイルアップロードエラー
```json
{
    "result": "error",
    "message": "ファイルのアップロードに失敗しました"
}
```

### Invalid File Type / 無効なファイルタイプ
```json
{
    "result": "error",
    "message": "サポートされていないファイル形式です"
}
