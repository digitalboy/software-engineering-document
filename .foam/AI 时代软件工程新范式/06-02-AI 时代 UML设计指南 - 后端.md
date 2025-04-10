# AI 时代 UML 设计指南 - 后端.md
**(聚焦领域模型、API 契约与业务逻辑，驱动代码生成)**

## 1. 引言：为后端自动化构建健壮的领域与服务蓝图

本指南是《AI 时代 UML 设计指南.md》（总则）在**后端开发（服务器端 API、业务服务、后台任务）**场景下的具体实践细则。在 AI 协同开发流程中，后端 UML 设计的目标是：

**利用 AI 辅助，基于 MVP 蓝图、相关技术试验结论和（来自前端 UML 的）API 消费需求，生成一套层次化、AI 友好的文本 UML (PlantUML/Mermaid) 规约。这份规约需精确定义后端的领域模型、核心业务逻辑、API 接口契约、数据持久化策略以及与其他内部/外部服务的交互，旨在为下游 AI 工具（特别是后端代码/测试/API 文档生成器）和开发者的实现工作提供坚实、无歧义的基础。**

**核心原则:** 严格遵循总则，同时**特别强调对领域模型、业务规则、状态转换、数据访问和 API 契约的精确建模，并明确排除底层基础设施配置、具体算法优化和非核心库的实现细节**。

## 2. 核心原则回顾 (后端侧重)

*   **AI 优先, 文本 UML:** 为机器解析和生成优化。
*   **分层细化 (L1-L3):** 从 API 边界 -> 内部模块/服务划分 -> 具体实现细节（类、序列、状态）。
*   **MECE:** 确保 API、服务、数据模型等职责清晰、无重叠、覆盖核心业务需求。
*   **基于验证:** 模型需反映技术试验（Web 框架、DB/ORM、认证库、外部 API 集成）的结论。
*   **聚焦核心结构与行为:** 建模 MVP 范围内的关键 API 端点、**业务实体**、**核心业务规则**、**数据访问逻辑**和**关键服务交互**。
*   **注释提供上下文:** 解释领域规则、设计决策、API 契约细节、性能考虑、事务边界、以及指向 Spikes 的引用。
*   **AI 生成，人工确认:** AI 提效，开发者把关领域逻辑、架构合理性和准确性。
*   **边界明确:** **不使用 UML 描述详细的数据库索引策略、SQL 优化、服务器配置、缓存具体实现、或复杂算法的内部步骤。** 这些应在编码或运维阶段处理。

## 3. 输入

*   《MVP 蓝图设计文档.md》 (定义后端需支持的功能、流程和技术设想)
*   《MVP 技术试验系列文档》 (提供已验证的后端技术细节、库用法、接口信息)
*   《AI UML 设计指南 - 前端.md》 (可选但推荐，特别是 L1/L2 用于理解 API 消费者)
*   (可选) 初步需求文档/用户故事集

## 4. 分层建模流程 (后端视角)

由“AI UML 架构师”工具或开发者与 LLM 交互执行：

**Level 0: 准备与分析**

*   AI 解析输入，重点理解后端的核心业务领域、需要提供的 API 能力、关键数据实体以及已验证的技术栈。

**Level 1: 系统上下文层 (定义 API 边界与外部依赖)**

*   **AI 生成 + 开发者审阅:**
    *   **主要图:** C4 风格上下文图 (文本 PlantUML/Mermaid)、(可选)高层用例图 (Actor 为 API 消费者)。
    *   **内容:** 清晰展示后端 API 与其主要消费者（前端 App, 其他服务）以及其依赖的关键外部系统（数据库, 第三方 API, 消息队列）之间的关系。
    *   **MECE 检查:** 是否覆盖所有主要外部交互方？API 边界是否清晰？外部依赖是否明确？

**Level 2: 关键模块/服务层 (划分后端内部架构)**

*   **AI 生成 + 开发者审阅:**
    *   **主要图:** 组件图或包图。
    *   **内容:** 基于架构风格（分层、微服务等）和领域逻辑，将后端系统分解为主要模块/服务：
        *   **分层示例:** API 层 (Routers/Controllers), 业务逻辑层 (Services), 数据访问层 (Repositories/DAOs), 领域模型 (Entities), 基础设施/工具 (Utils)。
        *   **微服务示例:** 用户服务 (UserService), 配方服务 (RecipeService), 认证服务 (AuthService), API 网关 (APIGateway)。
    *   清晰展示这些模块/服务之间的**主要依赖关系**和**暴露的关键接口**（逻辑层面）。
    *   **MECE 检查:** 模块/服务划分是否覆盖 L1 的能力？职责是否高内聚低耦合？关键依赖和接口是否表示？

**Level 3: 内部细节层 (深入模块/服务实现)**

*   **AI 生成 + 开发者审阅 (针对 L2 的每个关键模块/服务):**
    *   **类图 (用于结构):** **极其核心！**
        *   **领域模型 (Entities):** 精确定义核心业务对象的属性、数据类型（基于 Spike 验证）、关系、以及（可选的）关键业务方法签名。
        *   **数据传输对象 (DTOs):** 精确定义用于 API 请求和响应的数据结构，与前端/消费者契约一致。
        *   **服务类/接口:** 定义业务逻辑操作的**完整方法签名**（参数类型、返回类型 - 使用 DTOs）。
        *   **仓库类/接口 (Repositories/DAOs):** 定义数据访问操作的**完整方法签名**。
        *   **(可选) 值对象 (Value Objects):** 定义领域中具有特定含义且不可变的数据结构。
    *   **序列图 (用于交互流程):** **极其核心！**
        *   描绘**关键 API 请求**从接收到完成的**完整处理流程**，跨越不同层级（Router -> Service -> Repository -> DB）。
        *   展示**服务间的同步/异步交互**（如果是微服务）。
        *   清晰体现**事务边界**（逻辑上的开始和结束）。
        *   包含**基于 Spike 验证的关键库/框架用法**（如 ORM 调用、认证中间件逻辑）。
        *   可以包含**高层次的错误处理路径** (`alt Error Handling`)。
    *   **状态机图 (用于业务对象生命周期):** **极其核心！**
        *   精确描述关键**业务实体**（如订单 Order, 用户 User, 任务 Job）的状态、触发状态转换的事件/条件、以及转换时可能执行的动作 (Action)。
    *   **(可选) 活动图 (用于复杂业务规则):** 可视化包含复杂分支、合并、并发的业务流程或算法逻辑（高层次）。
    *   **(可选) 数据库 Schema 图 (类图风格):** 从领域模型生成逻辑数据库表结构、关系、主要约束 (PK, FK, Unique, Not Null)。
    *   **MECE 检查 (组件内):** 是否覆盖该模块/服务的核心职责？领域模型是否完整准确？API 契约是否清晰定义 (DTOs)？关键流程是否通过序列图覆盖？状态逻辑是否通过状态机图明确？

## 5. 关键 UML 类型与后端焦点

*   **上下文图 (L1):** 定义 API 边界、消费者和外部依赖。
*   **组件图/包图 (L2):** 描绘后端宏观架构（模块/层/服务）。
*   **类图 (L3):** **核心！用于精确定义领域模型 (Entities)、API 数据契约 (DTOs)、服务与数据访问接口。**
*   **序列图 (L3):** **核心！用于可视化请求处理流程、服务间交互、事务边界和关键技术集成点。**
*   **状态机图 (L3):** **核心！用于精确描述核心业务实体的生命周期和状态转换规则。**
*   **活动图 (L3 - 可选):** 用于复杂业务流程或规则的可视化。

## 6. 注释的最佳实践 (后端)

*   **领域模型:** 注释每个实体及其属性的业务含义、约束条件（如唯一性、长度限制）。
*   **DTOs:** 注释每个字段的含义、是否必需、格式要求，并与 API 文档保持一致。
*   **服务接口:** 注释每个方法的业务目的、前置条件、后置条件、主要业务规则、事务性要求、以及预期的错误场景。
*   **仓库接口:** 注释每个数据访问操作的目的和预期的数据返回。
*   **API 端点 (在序列图或相关注释中):** 明确请求方法、路径、参数、成功/错误响应码和 DTO。
*   **序列图:** 注释关键步骤的业务逻辑、事务边界、涉及的权限检查、对外部系统的调用等。
*   **状态机图:** 注释每个状态的业务含义、触发转换的事件/条件、以及转换动作的副作用。
*   **技术约束/决策:** `// Using optimistic locking for concurrency control.` `// JWT validation handled by middleware (see spike-auth-jwt.md).` `// Asynchronous task processing via Celery (details TBD in Phase 2).`

## 7. 输出：给后端 AI 和开发者的精确指令

*   一个层次化的包，包含各层级的 PlantUML/Mermaid 文件和详尽注释。
*   **主要用途:**
    *   驱动 AI 生成后端的 **API 路由/控制器骨架**。
    *   驱动 AI 生成 **服务类框架和业务逻辑基础**。
    *   驱动 AI 生成 **领域实体类 (POJOs/POCOs/etc.) 和 DTO 类**。
    *   驱动 AI 生成 **数据访问层接口和基于 ORM 的基础实现**。
    *   驱动 AI 生成 **数据库迁移脚本 (初步)**。
    *   作为生成 **API 文档 (如 OpenAPI Specification)** 的基础。
    *   为开发者提供清晰的**架构、领域模型、业务逻辑和 API 契约**。

## 8. 结论

本后端 UML 设计指南通过强调**分层、MECE、文本化和基于验证**的原则，并聚焦于**领域模型、API 契约和核心业务流程**，旨在利用 AI 高效生成精确、结构化的后端设计规约。这份规约为下游的 AI 代码生成和开发者的实现工作提供了坚实、无歧义的基础，是实现高质量、可维护后端系统并加速开发进程的关键步骤。