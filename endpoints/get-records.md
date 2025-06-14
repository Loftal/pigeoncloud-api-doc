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

- `condition`: 検索条件 / Search conditions
  - Format: JSON配列 / JSON array
  - 複数の条件を組み合わせた検索が可能 / Allows complex search with multiple conditions
  - 詳細は「検索条件について」を参照 / See "Search Conditions" section for details

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

### Using Search Conditions / 検索条件を使用
```http
POST /v1/record
Content-Type: application/json

{
  "table": "dataset__1",
  "condition": [
    {
      "field": "field__1",
      "condition": "eq",
      "value": "マスターユーザー",
      "and_or": "and"
    },
    {
      "field": "field__9",
      "condition": "gt",
      "value": 100,
      "and_or": "and"
    }
  ]
}
```

### Complex Search Conditions / 複雑な検索条件
```http
POST /v1/record
Content-Type: application/json

{
  "table": "dataset__1",
  "condition": [
    {
      "field": "field__1",
      "condition": "inc",
      "value": "ユーザー",
      "and_or": "and"
    },
    {
      "field": "field__7",
      "condition": "not_null",
      "and_or": "or"
    }
  ],
  "limit": 10,
  "offset": 0
}
```

## 検索条件について / Search Conditions

`condition`パラメータは、レコードを詳細にフィルタリングするためのJSON配列です。
The `condition` parameter is a JSON array for detailed record filtering.

### 条件演算子一覧 / Condition Operators

| 演算子 / Operator | 説明 / Description | 値の例 / Value Example |
|---|---|---|
| `eq` | 等しい / Equal | `"値"`, `123` |
| `neq` / `noteq` | 等しくない / Not equal | `"値"`, `123` |
| `gt` | より大きい / Greater than | `100`, `"2024-01-01"` |
| `lt` | より小さい / Less than | `100`, `"2024-12-31"` |
| `gt_ne` | 以上 / Greater than or equal | `100` |
| `lt_ne` | 以下 / Less than or equal | `100` |
| `null` | NULL値 / Is null | `null` (値は無視) |
| `not_null` | NULL以外 / Is not null | `null` (値は無視) |
| `inc` | 含む / Contains | `"検索文字"` |
| `notinc` | 含まない / Does not contain | `"除外文字"` |

### 検索条件の構造 / Search Condition Structure

各条件オブジェクトには以下のプロパティが含まれます：
Each condition object includes the following properties:

#### 必須プロパティ / Required Properties
- `field`: フィールド名 (例: `"field__1"`) / Field name
- `condition`: 条件演算子 / Condition operator
- `and_or`: 論理演算子 (`"and"` または `"or"`) / Logical operator

#### オプションプロパティ / Optional Properties
- `value`: 検索値 (文字列、数値、配列、真偽値、null) / Search value
- `type`: タイプ (`"field"` または `"select"`) / Type
- `subfield_a`: サブフィールド配列 / Subfield array
- `inc_table`: インクルードテーブル / Include table
- `inc_field`: インクルードフィールド / Include field
- `inc_filter_id`: インクルードフィルターID / Include filter ID
- `list_date_time_search_with_no_time`: 時間なしで日時検索 / Date-time search without time
- `use_dynamic_condition_value`: 動的条件値を使用 / Use dynamic condition value
- `date_relative_value`: 相対日付値 / Relative date value
- `variable_id`: 変数ID / Variable ID
- `use_variable`: 変数を使用するかどうか / Whether to use variable
- `select_condition`: 選択条件オブジェクト / Select condition object

### 使用例 / Usage Examples

#### 単一条件 / Single Condition
```json
[
  {
    "field": "field__1",
    "condition": "eq",
    "value": "検索値",
    "and_or": "and"
  }
]
```

#### 複数条件（AND） / Multiple Conditions (AND)
```json
[
  {
    "field": "field__1",
    "condition": "inc",
    "value": "ユーザー",
    "and_or": "and"
  },
  {
    "field": "field__2",
    "condition": "gt",
    "value": 100,
    "and_or": "and"
  }
]
```

#### 複数条件（OR） / Multiple Conditions (OR)
```json
[
  {
    "field": "field__1",
    "condition": "eq",
    "value": "管理者",
    "and_or": "and"
  },
  {
    "field": "field__1",
    "condition": "eq",
    "value": "ユーザー",
    "and_or": "or"
  }
]
```

#### 配列値での検索 / Array Value Search
```json
[
  {
    "field": "field__3",
    "condition": "eq",
    "value": ["値1", "値2", "値3"],
    "and_or": "and"
  }
]
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
