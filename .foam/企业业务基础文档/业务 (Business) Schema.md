# 业务 (Business) JSON Schema

**描述：**

本文件定义了 “业务 (Business)” 实体的 JSON Schema，用于描述企业所从事的、有组织的、持续性的、创造、交付和交换价值的活动集合。该 Schema 遵循《企业知识管理实体设计原则》，旨在确保数据的规范性和可扩展性。 本 Schema 严格遵循原子化原则，只定义 “业务” 实体本身的属性，避免存储任何与其他实体相关联的信息。

**JSON Schema:**

```json
{
  "type": "object",
  "properties": {
    "id": {
      "type": "string",
      "format": "uuid",
      "description": "业务的唯一标识符，使用 UUID 格式生成。"
    },
    "type": {
      "type": "string",
      "enum": ["业务"],
      "description": "实体的类型，固定为 ‘业务’。"
    },
    "name": {
      "type": "string",
       "minLength": 1,
      "maxLength": 255,
      "description": "业务的名称，例如 ‘电商销售’、‘软件开发’、‘咨询服务’。必须填写，并且长度在1到255个字符之间。"
    },
    "description": {
      "type": "string",
      "description": "对业务的详细描述，包括业务目标、核心活动和价值主张等。例如： ‘本业务主要通过在线平台销售商品，为客户提供便捷的购物体验’。"
    },
     "created_at": {
      "type": "string",
      "format": "date-time",
      "description": "业务记录的创建时间，使用 ISO 8601 格式表示，例如：'2024-01-26T10:00:00Z'。"
    },
    "updated_at": {
      "type": "string",
      "format": "date-time",
      "description": "业务记录的最后更新时间，使用 ISO 8601 格式表示，例如：'2024-01-26T12:00:00Z'。"
    },
     "tags": {
        "type": "array",
        "items": {
          "type": "string"
        },
         "description": "用于分类和检索的标签列表，例如： ‘#业务概念’，‘#基础概念’， ‘#核心业务’ 等。"
      }
  },
  "required": ["id", "type", "name", "description", "created_at", "updated_at"],
  "additionalProperties": false,
  "$schema": "http://json-schema.org/draft-07/schema#"
}
```

**属性说明：**

*   **`id` (string, uuid):** 业务的唯一标识符，使用 UUID 格式生成。
*   **`type` (string):** 实体的类型，固定为 "业务"。
*   **`name` (string):** 业务的名称，例如 “电商销售”、“软件开发”、“咨询服务”。必须填写，并且长度在1到255个字符之间。
*  **`description` (string):** 对业务的详细描述，包括业务目标、核心活动和价值主张等。例如：‘本业务主要通过在线平台销售商品，为客户提供便捷的购物体验’。
*   **`created_at` (string, date-time):** 业务记录的创建时间，使用 ISO 8601 格式表示，例如：`2024-01-26T10:00:00Z`。
*   **`updated_at` (string, date-time):** 业务记录的最后更新时间，使用 ISO 8601 格式表示，例如：`2024-01-26T12:00:00Z`。
*   **`tags` (array of strings):** 用于分类和检索的标签列表，例如：`#业务概念`，`#基础概念`， `#核心业务` 等。

**核心修改：**

*   **原子化：**  此 JSON Schema 只包含 “业务” 实体本身的属性，没有任何其他实体的信息，符合原子化原则。
*   **无关系属性：**  此 Schema 中没有包含任何和关系有关的属性，例如，没有流程id， 岗位id， 事件id 等等。
*   **遵循 JSON Schema 规范：** 所有属性都定义了明确的数据类型，格式，和约束。
*   **只描述 “类”：** 此 Schame 描述的是 “业务” 的类，而不是具体的 “业务” 实例。

**使用示例：**

```json
{
  "id": "a1b2c3d4-e5f6-7890-1234-567890abcdef",
  "type": "业务",
  "name": "电商销售",
  "description": "本业务主要通过在线平台销售商品，为客户提供便捷的购物体验。",
   "created_at": "2024-01-26T10:00:00Z",
  "updated_at": "2024-01-26T12:00:00Z",
  "tags": ["#业务概念", "#基础概念", "#核心业务"]
}
```

**总结：**

此 JSON Schema 提供了 “业务” 实体的规范定义，完全遵循了最新的《企业知识管理实体设计原则》， 特别是：坚持了原子化原则，避免了数据冗余，并且只描述了 “业务” 的 类。


