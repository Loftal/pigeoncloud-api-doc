# POST /v1/record (ファイルアップロード)

## Description
新しいレコードを作成し、ファイルをアップロードします。

## Single File Upload

### Request
```
POST /v1/record
Content-Type: multipart/form-data
```

### Parameters
- `table` (必須): データセットテーブル名
  - Format: `dataset__[id]` or table unique ID
  - Example: `dataset__1`, `test_dataset_unique_1`

- `field_name` (必須): アップロードするファイルフィールド
  - Type: file
  - Example: `field__39`

### Example (Python)
```python
def singleImgPost(self, table, field_name, file_path):
    fileDataBinary = open(file_path, 'rb').read()
    files = {field_name: (file_path, fileDataBinary, 'image/png')}
    data = {
        'table': table,
    }
    response = self.post(self.getApiUrl('record'), data=data, headers=self.getHeader(), files=files)
    return response
```

## Multiple Files Upload

### Request
```
POST /v1/record
Content-Type: multipart/form-data
```

### Parameters
- `table` (必須): データセットテーブル名
  - Format: `dataset__[id]` or table unique ID
  - Example: `dataset__1`, `test_dataset_unique_1`

- `field_name[]` (必須): アップロードする複数ファイルフィールド
  - Type: file array
  - Example: `field__39[]`

### Example (Python)
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

## Response
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

## Error Responses

### Invalid Table
```json
{
    "result": "error",
    "message": "tableは必須です"
}
```

### No Permission
```json
{
    "result": "error",
    "message": "権限がありません"
}
```

### File Upload Error
```json
{
    "result": "error",
    "message": "ファイルのアップロードに失敗しました"
}
```

### Invalid File Type
```json
{
    "result": "error",
    "message": "サポートされていないファイル形式です"
}
