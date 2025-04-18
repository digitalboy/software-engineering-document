## AI UML 架构师 - 后端设计指南 (服务器).md

### 1. 引言：为后端 AI 构建健壮的领域与 API 蓝图

本指南是《AI UML 架构师 - 总则》在**后端开发（服务器端 API、服务、后台任务）**场景下的具体应用。其目标是利用 AI，将需求和 MVP 验证成果转化为一套层次化、结构清晰、面向 AI 的文本 UML 规约，**专注于后端的领域模型、业务逻辑、API 契约、数据持久化和与其他服务的交互**。

遵循总则，本文档旨在为下游 AI 工具（代码生成器、API 文档生成器、测试框架生成器）提供精确的输入，同时明确排除底层基础设施配置和过于细节的算法实现。

### 2. 核心原则回顾 (后端侧重)

*   **AI 优先, 文本 UML (PlantUML/Mermaid):** 为机器优化。
*   **分层细化 (L1-L3):** 从 API 边界到内部模块和服务。
*   **MECE:** 确保 API、服务、数据模型的职责清晰且覆盖核心业务。
*   **聚焦核心结构与行为:** 建模 MVP 范围内的关键 API 端点、业务实体、核心业务规则、数据访问逻辑和外部服务集成。
*   **明确边界:** 本指南**不涉及**具体的数据库优化（如索引策略细节）、基础设施部署配置（如 K8s manifests）、复杂的算法内部步骤（除非其交互是关键）、或非核心的通用库实现细节。

### 3. 输入

同总则：需求描述、MVP 核心逻辑、**后端相关的 Spike 文档** (如 Web 框架选型、DB/ORM 验证、认证机制实现、外部 API 集成验证)、(可选) 后端核心示例代码。

### 4. 分层工作流 (后端视角)

*   **Level 0: 摄入与分析:** AI 理解需求、MVP 成果，识别后端服务的核心职责和主要消费者。
*   **Level 1: 系统上下文层:**
    *   **AI 生成:**
        *   **用例图 (可选):** Actor 可以是**前端应用、移动 App、其他后端服务、定时任务触发器**等，系统为**后端服务本身**，用例为提供的核心能力（如“管理用户数据”、“处理订单”）。
        *   **上下文图 (C4 风格):** 展示**消费者**（前端、其他服务）与**后端服务**的交互，以及后端服务与**依赖的外部系统**（数据库、第三方 API、消息队列）的交互。
    *   **验证:** 是否清晰定义了 API 边界和外部依赖？
*   **Level 2: 关键模块/服务层:**
    *   **AI 分析:** 识别后端内部的主要逻辑分区，可以是按**领域/功能模块**（如用户管理、配方管理）、按**技术分层**（如 API 层、业务逻辑层、数据访问层），或在微服务架构中代表**单个服务**。
    *   **AI 生成:**
        *   **组件图/包图:** 展示这些主要模块/服务及其**内部依赖关系**和**对外暴露的接口**（例如，API 模块依赖业务逻辑模块，业务逻辑模块依赖数据访问模块）。
    *   **验证:** 模块/服务划分是否合理（高内聚低耦合）？职责是否清晰？是否覆盖 L1 的能力？关键依赖是否表示？
*   **Level 3: 内部细节层 (模块/服务内部):**
    *   **AI 分析:** 针对 L2 的**每个关键模块/服务**进行细化。
    *   **AI 生成 (聚焦领域、逻辑、数据和 API):**
        *   **类图 (用于结构):** **极其重要！**
            *   **领域模型 (Entities):** 定义核心业务对象的属性、关系（关联、聚合、继承）。
            *   **数据传输对象 (DTOs):** 定义 API 请求/响应的数据结构。
            *   **服务类/接口:** 定义业务逻辑操作的**签名**。
            *   **仓库类/接口 (Repositories/DAOs):** 定义数据访问操作的**签名**。
        *   **序列图 (用于交互):** **非常重要！** 描绘关键 API 请求的处理流程：
            *   `API Controller/Router -> Service -> Repository -> Database`
            *   `Service A -> Service B (同步/异步)`
            *   `Service -> External API`
            *   展示事务边界、关键业务规则的应用、错误处理路径（高层）。
        *   **状态机图 (用于业务对象生命周期):** **非常重要！** 描述核心**业务实体**（如订单 Order、用户 User、任务 Job）的复杂状态转换逻辑。
        *   **(可选) 活动图 (用于复杂流程):** 可视化包含多个步骤、分支、合并的复杂业务规则或编排逻辑。
        *   **(可选) 数据库 Schema 图 (类图风格):** 从领域模型映射出逻辑数据库表结构、关系、主要约束（如 PK, FK, Unique）。
    *   **验证:** 类图是否准确定义领域模型和 API 数据结构？序列图是否覆盖关键业务流程和交互？状态机是否正确描述核心实体生命周期？

### 5. 关键 UML 类型与后端焦点

*   **上下文图 (L1):** 定义 API 边界和外部依赖。
*   **组件图/包图 (L2):** 描绘后端代码库的宏观架构（模块/层/服务）。
*   **类图 (L3):** **核心！** 用于定义**领域模型、DTOs、服务接口、数据访问接口**。
*   **序列图 (L3):** **核心！** 用于可视化**API 请求处理、服务间交互、关键业务逻辑流**。
*   **状态机图 (L3):** **核心！** 用于描述**业务实体**的生命周期。
*   **活动图 (L3 - 可选):** 用于复杂业务流程。

### 6. 示例调整：“自酿啤酒社群”APP 后端

(参考《总则》中已修正的后端 UML 示例，如 `classes_core_models.puml`, `classes_services_routers.puml`, `sequence_create_recipe.puml`。它们已经很好地体现了后端的建模重点。)

*   **重点强调:** 生成的 UML **不会**包含具体的 SQL 查询优化、缓存实现细节、框架内部的底层机制、或详细的错误日志格式。序列图中的错误处理可能是高层次的表示（如 `alt Error Occurs`），具体的异常类和处理代码在实现阶段添加。

### 7. 输出：给后端 AI 的精确指令

*   一个层次化的包，包含各层级的 PlantUML/Mermaid 文件和注释。
*   **主要用途:** 驱动生成后端的**API 路由/控制器骨架、服务类框架、领域实体类、DTO 类、数据访问层接口/基本实现 (基于 ORM)、数据库迁移脚本（初步）、API 文档（如 OpenAPI spec 的基础）**。

### 8. 结论

本后端设计指南利用 AI 和分层文本 UML，为自动化生成后端服务的核心结构、业务逻辑框架和 API 定义提供了强大支持。通过明确建模范围，聚焦于领域模型、核心流程和接口契约，它为下游 AI 代码生成工具提供了高质量的输入，显著加速后端开发进程，同时保证了设计的结构化和一致性。
