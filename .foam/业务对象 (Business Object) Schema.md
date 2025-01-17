# 业务对象 (Business Object) JSON Schema

**描述：**

本文件定义了 “业务对象 (Business Object)” 实体的 JSON Schema，用于描述企业在运营过程中需要管理的、具有特定属性的实体或概念。 “业务对象” 是一个抽象的父类，具体的业务对象，例如客户，产品，订单等，应该继承此 Schame 的定义。该 Schema 遵循《企业知识管理实体设计原则》，旨在确保数据的规范性和可扩展性。 本 Schema 严格遵循原子化原则，只定义 “业务对象” 实体本身的属性，避免存储任何与其他实体相关联的信息。

**JSON Schema:**

```json
{
  "type": "object",
  "properties": {
    "id": {
      "type": "string",
      "format": "uuid",
      "description": "业务对象的唯一标识符，使用 UUID 格式生成。"
    },
    "type": {
      "type": "string",
      "enum": ["业务对象"],
      "description": "实体的类型，固定为 ‘业务对象’。"
    },
    "name": {
      "type": "string",
      "minLength": 1,
      "maxLength": 255,
      "description": "业务对象的名称，例如 ‘客户’、‘产品’、‘订单’。必须填写，并且长度在1到255个字符之间。"
    },
     "description": {
      "type": "string",
      "description": "对业务对象的详细描述，包括业务对象的特点和作用。例如：‘本业务对象描述了客户的属性和行为，包括客户的姓名、地址、联系方式等’。"
    },
    "created_at": {
      "type": "string",
      "format": "date-time",
       "description": "业务对象记录的创建时间，使用 ISO 8601 格式表示，例如：'2024-01-26T10:00:00Z'。"
    },
    "updated_at": {
      "type": "string",
      "format": "date-time",
       "description": "业务对象记录的最后更新时间，使用 ISO 8601 格式表示，例如：'2024-01-26T12:00:00Z'。"
    },
    "tags": {
      "type": "array",
      "items": {
        "type": "string"
      },
       "description": "用于分类和检索的标签列表，例如： ‘#业务概念’， ‘#基础概念’，‘#业务对象’， ‘#客户’， ‘#产品’ 等。"
    },
    "object_type": {
      "type": "string",
      "description": "业务对象的类型，例如，客户(customer)，产品(product)，订单(order)。",
       "minLength": 1
    },
    "attributes": {
      "type": "object",
      "description": "业务对象的属性，使用键值对的形式来表示。",
       "additionalProperties": true
    }
  },
  "required": ["id", "type", "name", "description", "created_at", "updated_at", "object_type"],
  "additionalProperties": false,
   "$schema": "http://json-schema.org/draft-07/schema#"
}
```

**属性说明：**

*   **`id` (string, uuid):** 业务对象的唯一标识符，使用 UUID 格式生成。
*   **`type` (string):** 实体的类型，固定为 “业务对象”。
*   **`name` (string):** 业务对象的名称，例如 “客户”、“产品”、“订单”。必须填写，并且长度在1到255个字符之间。
*   **`description` (string):** 对业务对象的详细描述，包括业务对象的特点和作用。例如：‘本业务对象描述了客户的属性和行为，包括客户的姓名、地址、联系方式等’。
*   **`created_at` (string, date-time):** 业务对象记录的创建时间，使用 ISO 8601 格式表示，例如：`2024-01-26T10:00:00Z`。
*   **`updated_at` (string, date-time):** 业务对象记录的最后更新时间，使用 ISO 8601 格式表示，例如：`2024-01-26T12:00:00Z`。
*   **`tags` (array of strings):** 用于分类和检索的标签列表，例如：`"#业务概念"`，`"#基础概念"`， `"#业务对象"`， `"客户"`， `"#产品"` 等。
*  **`object_type` (string):** 业务对象的类型，例如，`customer`， `product`，`order`。 必须填写，并且长度在1个字符以上。
*   **`attributes` (object):** 业务对象的属性，使用键值对的形式来表示。

**核心修改：**

*   **原子化：** 此 JSON Schema 只包含 “业务对象” 实体本身的属性，没有任何其他实体的信息，符合原子化原则。
*   **无关系属性：** 此 Schema 中没有包含任何和关系有关的属性， 例如，没有 流程id， 岗位id， 事件id 等等。
*   **遵循 JSON Schema 规范：** 所有属性都定义了明确的数据类型，格式，和约束。
*    **只描述 “类”：** 此 Schame 描述的是 “业务对象” 的类，而不是具体的 “业务对象” 实例。
* **使用 `additionalProperties: true`**: 允许动态添加属性，但是需要在子类中使用 `properties` 和 `required` 明确定义。

**示例 JSON Schema (继承自 `业务对象` 的子类):**

1.  **客户 (Customer) 示例:**

```json
// 继承 "业务对象"
{
  "type": "object",
  "properties": {
    "id": { "$ref": "#/properties/id" },
    "type": { "$ref": "#/properties/type", "enum": ["业务对象"]},
    "name": { "$ref": "#/properties/name" },
    "description": { "$ref": "#/properties/description"},
      "created_at": { "$ref": "#/properties/created_at" },
     "updated_at": { "$ref": "#/properties/updated_at" },
    "tags": { "$ref": "#/properties/tags" },
    "object_type": { "type": "string", "enum":["customer"], "description":"业务对象的类型，必须是 customer"},
    "attributes": {
        "type": "object",
          "description": "客户的属性，使用键值对的形式来表示。",
           "properties":{
                "customer_id": { "type": "string", "description": "客户的唯一标识符" },
                "name": { "type": "string", "description": "客户的姓名" },
                 "email": { "type": "string", "format": "email", "description": "客户的电子邮件" },
                 "phone": { "type": "string", "description": "客户的电话号码" },
                   "address": { "type": "string", "description": "客户的地址" }
           },
         "required": ["customer_id","name"]
      }
  },
  "required": ["id", "type", "name", "description", "created_at", "updated_at", "object_type", "attributes"],
   "additionalProperties": false,
   "$schema": "http://json-schema.org/draft-07/schema#"
}
```

2.  **产品 (Product) 示例:**

```json
// 继承 "业务对象"
{
  "type": "object",
  "properties": {
    "id": { "$ref": "#/properties/id" },
     "type": { "$ref": "#/properties/type", "enum": ["业务对象"]},
    "name": { "$ref": "#/properties/name" },
       "description": { "$ref": "#/properties/description"},
      "created_at": { "$ref": "#/properties/created_at" },
     "updated_at": { "$ref": "#/properties/updated_at" },
    "tags": { "$ref": "#/properties/tags" },
     "object_type": { "type": "string", "enum":["product"], "description":"业务对象的类型，必须是 product"},
     "attributes": {
         "type": "object",
           "description": "产品的属性，使用键值对的形式来表示。",
           "properties":{
                "product_id": { "type": "string", "description": "产品的唯一标识符" },
                "product_name": { "type": "string", "description": "产品的名称" },
                "price": { "type": "number", "description": "产品的价格" },
                 "description": { "type": "string", "description": "产品的详细描述" },
                   "stock": { "type": "integer", "description": "产品的库存" }
           },
         "required": ["product_id","product_name", "price"]
      }
  },
   "required": ["id", "type", "name", "description", "created_at", "updated_at", "object_type", "attributes"],
     "additionalProperties": false,
   "$schema": "http://json-schema.org/draft-07/schema#"
}
```

3.  **订单 (Order) 示例:**

```json
// 继承 "业务对象"
{
    "type": "object",
  "properties": {
     "id": { "$ref": "#/properties/id" },
      "type": { "$ref": "#/properties/type", "enum": ["业务对象"]},
    "name": { "$ref": "#/properties/name" },
     "description": { "$ref": "#/properties/description"},
      "created_at": { "$ref": "#/properties/created_at" },
     "updated_at": { "$ref": "#/properties/updated_at" },
    "tags": { "$ref": "#/properties/tags" },
       "object_type": { "type": "string", "enum":["order"], "description":"业务对象的类型，必须是 order"},
     "attributes": {
         "type": "object",
           "description": "订单的属性，使用键值对的形式来表示。",
            "properties": {
                "order_id": { "type": "string", "description": "订单的唯一标识符" },
                "customer_id": { "type": "string", "description": "客户的唯一标识符，引用客户业务对象" },
                "order_date": { "type": "string", "format":"date-time", "description": "订单的创建日期" },
                 "total_amount": { "type": "number", "description": "订单的总金额" },
                "order_items": {
                  "type": "array",
                   "description": "订单的商品明细",
                  "items": {
                      "type":"object",
                      "properties":{
                            "product_id": { "type": "string", "description": "产品的唯一标识符，引用产品业务对象" },
                           "quantity": { "type": "integer", "description": "购买的数量" },
                            "price": { "type": "number", "description": "购买时的单价" }
                       },
                    "required":["product_id", "quantity", "price"]
                    }
                 }
             },
           "required": ["order_id","customer_id", "order_date", "total_amount", "order_items"]
      }
  },
   "required": ["id", "type", "name", "description", "created_at", "updated_at", "object_type", "attributes"],
    "additionalProperties": false,
    "$schema": "http://json-schema.org/draft-07/schema#"
}
```

**说明：**

*   **继承：** 具体的 “客户” ， “产品” 和 “订单”  都通过 `$ref` 继承了 `业务对象` 的公共属性。
*  **`object_type` 属性：**  在具体的对象中， `object_type` 使用 `enum` 进行了限定。
*   **`attributes` 属性：**  `attributes` 属性都使用 object 类型， 并增加了 `properties` 和 `required` 来定义其内部的结构。
*  **`additionalProperties: false`**: 明确指出不允许添加额外的属性，保证结构的严谨性。

**总结：**

此 JSON Schema 定义了 “业务对象” 实体的规范定义， 完全遵循了最新的《企业知识管理实体设计原则》， 特别是：坚持了原子化原则，避免了数据冗余，并且只描述了 “业务对象” 的 类。

