# PUT /v1/update_record (ファイルアップロード)

## Description
既存のレコードを更新し、ファイルをアップロードします。

## Single File Upload

### Request
```
POST /v1/update_record
Content-Type: multipart/form-data
```

### Parameters
- `table_id` (必須): データセットテーブルID
  - Example: `5`

- `data[0][id]` (必須): 更新するレコードID
  - Example: `1`

- `notify` (任意): 通知を送信するか
  - Type: boolean
  - Default: true

- `field_name` (必須): アップロードするファイルフィールド
  - Type: file
  - Example: `field__39`

### Example (Python)
```python
def singleImgPut(self, table_id, field_name, id, file_path):
    #if file not exist return False
    if not os.path.exists(file_path):
        raise Exception(f'File not found: {file_path}')
    fileDataBinary = open(file_path, 'rb').read()
    files = {}
    files[field_name+'[]'] = (file_path, fileDataBinary, 'image/png')
    data = {
        'table_id': table_id,
        'data[0][id]': id,
        'notify': False,
    }
    response = self.post(self.getApiUrl('update_record'), data=data, headers=self.getHeader(), files=files)
    return response
```

## Multiple Files Upload

### Request
```
POST /v1/update_record
Content-Type: multipart/form-data
```

### Parameters
- `table_id` (必須): データセットテーブルID
  - Example: `5`

- `data[0][id]` (必須): 更新するレコードID
  - Example: `1`

- `notify` (任意): 通知を送信するか
  - Type: boolean
  - Default: true

- `data[0][field_name][index][order]` (任意): 画像の順序
  - Example: `data[0][field__39][0][order]`

- `field_name[0][index]` (必須): アップロードする複数ファイルフィールド
  - Type: file array
  - Example: `field__39[0][0]`

### Example (Python)
```python
def multiImgPut(self, table, field_name, id, file_path_a, delete_old_images=False):
    # 現在のレコードの画像データを取得
    params = {
        "table_id": table,
        "condition": [
            {
                "and_or": "and",
                "field": "id",
                "condition": "eq",
                "value": id
            }
        ],
        "offset": 0,
        "limit": 1,
        "order": "id desc",
        "norify": False
    }

    old_record_data = self.get_pigeon_data(params)
    if old_record_data is None:
        print("Error: No record found.")
        return

    # 既存の画像IDと順序を取得
    old_images = old_record_data['data'][0]['raw_data'][f'{field_name}_ids']
    image_order = len(old_images)

    # 既存の画像を削除するためのデータ構造を準備
    delete_images = {}
    if delete_old_images:
        delete_images = {
            "table_id": table,
            "notify": False,
            f"data[0][id]": id,
        }
        for index, image_id in enumerate(old_images):
            delete_images[f"data[0][{field_name}][{index}][id]"] = image_id
            delete_images[f"data[0][{field_name}][{index}][_delete]"] = "true"

        response = self.post(self.getApiUrl('update_record'), data=delete_images, headers=self.getHeader())
    
    # 新しい画像ファイルを準備
    files = {}
    images = {}
    for index, file_path in enumerate(file_path_a):
        fileDataBinary = open(file_path, 'rb').read()
        # 画像の順序をインクリメント
        image_order += 1

        # imagesとして新しい画像の順序を記録
        images[f"data[0][{field_name}][{index}][order]"] = image_order

        # ファイルをリクエストに追加
        files[f"{field_name}[0][{index}]"] = (file_path, fileDataBinary, 'image/png')

    # 最終的なデータ構造を作成
    data = {
        "table_id": table,
        f"data[0][id]": id,
        "notify": False,
        **images  # 順序付きの画像データ
    }

    # 画像データとファイルをPOSTリクエストで送信
    response = self.post(self.getApiUrl('update_record'), data=data, headers=self.getHeader(), files=files)
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
    "message": "table_idは必須です"
}
```

### No Permission
```json
{
    "result": "error",
    "message": "権限がありません"
}
```

### Record Not Found
```json
{
    "result": "error",
    "message": "レコードが見つかりません"
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
