# 岗位 (Job Role) JSON Schema

**描述：**

本文件定义了 “岗位 (Job Role)” 实体的 JSON Schema，用于描述企业组织结构中为实现特定业务目标而设立的、由一个人或多个人承担特定职责的角色。该 Schema 遵循《企业知识管理实体设计原则》，旨在确保数据的规范性和可扩展性。 本 Schema 严格遵循原子化原则，只定义 “岗位” 实体本身的属性，避免存储任何与其他实体相关联的信息。

**JSON Schema:**

```json
{
  "type": "object",
  "properties": {
    "id": {
      "type": "string",
      "format": "uuid",
      "description": "岗位的唯一标识符，使用 UUID 格式生成。"
    },
    "type": {
      "type": "string",
      "enum": ["岗位"],
      "description": "实体的类型，固定为 ‘岗位’。"
    },
    "name": {
      "type": "string",
        "minLength": 1,
      "maxLength": 255,
      "description": "岗位的名称，例如 ‘软件工程师’、‘项目经理’、‘销售代表’。必须填写，并且长度在1到255个字符之间。"
    },
    "description": {
      "type": "string",
      "description": "对岗位的详细描述，包括岗位职责和目标。例如： ‘负责软件的开发和维护’。"
    },
    "created_at": {
      "type": "string",
      "format": "date-time",
      "description": "岗位记录的创建时间，使用 ISO 8601 格式表示，例如：'2024-01-26T10:00:00Z'。"
    },
    "updated_at": {
      "type": "string",
      "format": "date-time",
      "description": "岗位记录的最后更新时间，使用 ISO 8601 格式表示，例如：'2024-01-26T12:00:00Z'。"
    },
     "tags": {
        "type": "array",
        "items": {
          "type": "string"
        },
         "description": "用于分类和检索的标签列表，例如： ‘#业务概念’，‘#基础概念’， ‘#岗位’ ， ‘#软件工程’ 等。"
      },
     "department": {
        "type": "string",
        "minLength": 1,
        "description": "岗位所属的部门， 例如 ‘研发部’， ‘销售部’等。"
    },
      "responsibilities": {
      "type": "array",
       "items": {
          "type": "string",
            "description": "岗位职责列表"
        },
          "minItems":1,
         "description": "岗位职责列表，必须至少有一个职责。"
    }
  },
  "required": ["id", "type", "name", "description", "created_at", "updated_at", "responsibilities", "department"],
    "additionalProperties": false,
    "$schema": "http://json-schema.org/draft-07/schema#"
}