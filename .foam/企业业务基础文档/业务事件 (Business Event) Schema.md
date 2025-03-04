# 业务事件 (Business Event) JSON Schema

**描述：**

本文件定义了 “业务事件 (Business Event)” 实体的 JSON Schema，用于描述企业运营过程中发生的、对业务对象的状态、属性或生命周期产生显著影响的可观察变化或发生。“业务事件” 是一个抽象的父类，具体的业务事件，例如订单创建事件、付款事件、发货事件等，应该继承此 Schema 的定义。该 Schema 遵循《企业知识管理实体设计原则》，旨在确保数据的规范性和可扩展性。 本 Schema 严格遵循原子化原则，只定义 “业务事件” 实体本身的属性，避免存储任何与其他实体相关联的信息。

**JSON Schema:**

```json
{
  "type": "object",
  "properties": {
    "id": {
      "type": "string",
      "format": "uuid",
      "description": "业务事件的唯一标识符，使用 UUID 格式生成。"
    },
    "type": {
      "type": "string",
      "enum": ["业务事件"],
       "description": "实体的类型，固定为 ‘业务事件’。"
    },
    "name": {
      "type": "string",
      "minLength": 1,
      "maxLength": 255,
      "description": "业务事件的名称，例如 ‘订单创建’、‘付款成功’、‘发货完成’。必须填写，并且长度在1到255个字符之间。"
    },
    "description": {
      "type": "string",
      "description": "对业务事件的详细描述，包括事件的触发条件，以及对业务对象的影响。例如： ‘当客户在在线平台成功下单后，触发订单创建事件，并影响订单的状态’。"
    },
     "created_at": {
      "type": "string",
      "format": "date-time",
      "description": "业务事件记录的创建时间，使用 ISO 8601 格式表示，例如：'2024-01-26T10:00:00Z'。"
    },
    "updated_at": {
      "type": "string",
      "format": "date-time",
      "description": "业务事件记录的最后更新时间，使用 ISO 8601 格式表示，例如：'2024-01-26T12:00:00Z'。"
    },
    "tags": {
      "type": "array",
      "items": {
        "type": "string"
      },
       "description": "用于分类和检索的标签列表，例如： ‘#业务概念’， ‘#二级概念’，‘#业务事件’， ‘#订单事件’ 等。"
    },
    "event_type": {
      "type": "string",
      "description": "业务事件的类型， 例如： ‘订单创建’， ‘付款成功’， ‘发货完成’。",
       "minLength": 1
    }
  },
    "required": ["id", "type", "name", "description", "created_at", "updated_at", "event_type"],
     "additionalProperties": false,
      "$schema": "http://json-schema.org/draft-07/schema#"
}
```

**属性说明：**

*   **`id` (string, uuid):** 业务事件的唯一标识符，使用 UUID 格式生成。
*   **`type` (string):** 实体的类型，固定为 “业务事件”。
*   **`name` (string):** 业务事件的名称，例如 “订单创建”、“付款成功”、“发货完成”。必须填写，并且长度在1到255个字符之间。
*   **`description` (string):** 对业务事件的详细描述，包括事件的触发条件，以及对业务对象的影响。 例如： ‘当客户在在线平台成功下单后，触发订单创建事件，并影响订单的状态’。
*   **`created_at` (string, date-time):** 业务事件记录的创建时间，使用 ISO 8601 格式表示，例如：`2024-01-26T10:00:00Z`。
*   **`updated_at` (string, date-time):** 业务事件记录的最后更新时间，使用 ISO 8601 格式表示，例如：`2024-01-26T12:00:00Z`。
*   **`tags` (array of strings):** 用于分类和检索的标签列表，例如：`#业务概念`，`#二级概念`，`#业务事件`， `"#订单事件"` 等。
*   **`event_type` (string):** 业务事件的类型， 例如：  `订单创建`， `付款成功`， `发货完成`。必须填写，并且长度在1个字符以上。

**核心修改：**

*   **原子化：** 此 JSON Schema 只包含 “业务事件” 实体本身的属性，没有任何其他实体的信息，符合原子化原则。
*   **无关系属性：** 此 Schema 中没有包含任何和关系有关的属性，例如，没有流程id， 岗位id， 业务对象id，时间 id 等等。
*   **遵循 JSON Schema 规范：** 所有属性都定义了明确的数据类型，格式，和约束。
*   **只描述 “类”：** 此 Schame 描述的是 “业务事件” 的类，而不是具体的 “业务事件” 实例。
*   **移除了 `affected_objects` 属性：**  影响的业务对象，应该通过关系来表达，而不应该作为属性来存储。
*  **移除了 `time_id` 属性：**  时间应该通过关系属性来引用时间节点，而不是直接包含在事件的属性中。

**示例 JSON Schema (继承自 `业务事件` 的子类)：**

1.  **订单创建事件 (Order Created Event) 示例：**

```json
// 继承 "业务事件"
{
  "type": "object",
  "properties": {
     "id": { "$ref": "#/properties/id" },
      "type": { "$ref": "#/properties/type", "enum": ["业务事件"]},
    "name": { "$ref": "#/properties/name" , "default": "订单创建"},
        "description": { "$ref": "#/properties/description", "default": "当客户成功下单后，触发订单创建事件。"},
      "created_at": { "$ref": "#/properties/created_at" },
     "updated_at": { "$ref": "#/properties/updated_at" },
     "tags": { "$ref": "#/properties/tags" },
    "event_type": { "type": "string", "enum": ["订单创建"], "description": "业务事件的类型，必须是订单创建"}
  },
   "required": ["id", "type", "name", "description", "created_at", "updated_at", "event_type"],
    "additionalProperties": false,
    "$schema": "http://json-schema.org/draft-07/schema#"
}
```

2.  **付款成功事件 (Payment Succeeded Event) 示例:**

```json
// 继承 "业务事件"
{
 "type": "object",
  "properties": {
     "id": { "$ref": "#/properties/id" },
      "type": { "$ref": "#/properties/type", "enum": ["业务事件"]},
    "name": { "$ref": "#/properties/name" , "default": "付款成功"},
        "description": { "$ref": "#/properties/description", "default": "当客户成功支付订单后，触发付款成功事件。"},
      "created_at": { "$ref": "#/properties/created_at" },
     "updated_at": { "$ref": "#/properties/updated_at" },
     "tags": { "$ref": "#/properties/tags" },
    "event_type": { "type": "string", "enum": ["付款成功"], "description": "业务事件的类型，必须是付款成功"}
  },
   "required": ["id", "type", "name", "description", "created_at", "updated_at", "event_type"],
    "additionalProperties": false,
     "$schema": "http://json-schema.org/draft-07/schema#"
}
```

3.  **发货完成事件 (Shipment Completed Event) 示例:**

```json
// 继承 "业务事件"
{
  "type": "object",
  "properties": {
      "id": { "$ref": "#/properties/id" },
      "type": { "$ref": "#/properties/type", "enum": ["业务事件"]},
    "name": { "$ref": "#/properties/name" , "default": "发货完成"},
        "description": { "$ref": "#/properties/description", "default": "当货物完成发货后，触发发货完成事件。"},
      "created_at": { "$ref": "#/properties/created_at" },
     "updated_at": { "$ref": "#/properties/updated_at" },
      "tags": { "$ref": "#/properties/tags" },
    "event_type": { "type": "string", "enum": ["发货完成"], "description": "业务事件的类型，必须是发货完成"}
  },
   "required": ["id", "type", "name", "description", "created_at", "updated_at", "event_type"],
   "additionalProperties": false,
    "$schema": "http://json-schema.org/draft-07/schema#"
}
```

**说明：**

*   **继承：** 具体的 “订单创建事件” ， “付款成功事件” 和 “发货完成事件” 都通过 `$ref` 继承了 `业务事件` 的公共属性。
*   **`event_type` 属性：** 在具体的事件中，`event_type` 使用 `enum` 进行了限定。
*    **`additionalProperties: false`**: 明确指出不允许添加额外的属性，保证结构的严谨性。
*   **`default` 属性：** 对于一些子类特定的属性，例如 “name”， 和 “description”， 可以使用 `default` 提供默认值。
*  **移除了 `affected_objects` 属性：**  影响的业务对象，应该通过关系来表达， 而不应该作为属性存储在节点中。
*   **移除了  `time_id` 属性**: 时间应该通过关系属性来表达， 而不应该作为属性存储在节点中。

**总结：**

此 JSON Schema 定义了 “业务事件” 实体的规范定义，完全遵循了最新的《企业知识管理实体设计原则》，特别是：坚持了原子化原则，避免了数据冗余，并且只描述了 “业务事件” 的 类。


