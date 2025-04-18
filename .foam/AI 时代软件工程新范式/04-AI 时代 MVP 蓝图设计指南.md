# AI 时代 MVP 蓝图设计指南.md
**(工程化视角，定义范围与目标)**

## 1. 引言：从概念到可执行蓝图的转化

在 AI 驱动的开发流程中，“MVP 蓝图设计”阶段是连接早期概念（来自需求定义和视觉原型）与后续技术实践（技术试验、UML 规约、编码）的关键桥梁。它的核心目标不再是编写一份面面俱到的传统需求文档，而是：

**利用 AI 辅助，基于已有的需求信息（通过“问题链”等方式获取，包含初步用户画像和故事）和高保真视觉原型 (HTML/CSS)，快速生成一份结构化、工程化的 MVP 蓝图。这份蓝图需清晰定义 MVP 的核心范围、功能优先级、关键流程和高层技术设想，旨在精确指导后续的 UML 设计和第一阶段编码实现。**

本文档强调从已有输入中**提炼和固化**关键信息，使之更适合技术评估和工程实现，其主要目的是**定义“做什么”(What)和初步的“打算怎么做”(Initial How)**。

## 2. 核心原则

1.  **工程化陈述 (Engineering-Focused Language):** 使用精确、结构化的语言描述功能和流程。
2.  **聚焦 MVP 核心 (Laser Focus on MVP Core):** 严格限定范围，对功能进行优先级排序。
3.  **原型驱动输入 (Prototype-Informed Input):** 高保真视觉原型提供界面结构信息，使功能描述更具体。
4.  **指导后续设计与开发 (Guiding Design & Development):** 蓝图是 AI UML 架构师和 Phase 1 编码的主要输入，定义了实现的目标和边界。
5.  **AI 辅助提炼与结构化 (AI Assists Refinement & Structuring):** 利用 AI 提取信息、归纳总结并生成结构化文档。
6.  **快速产出，迭代基础 (Rapid Output for Iteration):** 快速生成蓝图，作为后续迭代的基础。

## 3. 输入

*   **初步需求文档/用户故事集:** (来自“问题链”阶段)
*   **视觉高保真原型 (HTML/CSS):** (来自“AI 辅助视觉高保真原型”阶段)
*   **开发者的初步技术想法 (可选)。**

## 4. 核心步骤 (AI 协同)

1.  **AI 解析输入，识别核心要素:** (同前) AI 解析需求和原型，提取用户画像、痛点、界面、初步功能点和数据实体。AI 反馈初步识别结果。
2.  **确认/精炼用户画像与核心问题:** (同前) 开发者审阅并确认 MVP 的目标用户和核心问题。
3.  **定义并排序 MVP 核心功能集 (工程化视角):** (同前)
    *   开发者严格筛选必需功能 (Must-have)，参照 MoSCoW。
    *   使用工程化语言描述功能。
    *   在 Must-have 内部进行 P0, P1... 排序。
4.  **勾勒关键用户流程 (结合原型):** (同前)
    *   选择核心流程。
    *   参照原型，描述包含界面、操作、预期响应的步骤。
5.  **列出高层技术考虑/设想 (Identify High-Level Technical Considerations/Assumptions):**
    *   **开发者主导 + AI 辅助:**
        *   基于 MVP 功能集和关键流程，列出**初步的、高层次的技术栈设想或关键技术组件**。
        *   **目的:** 记录下当前阶段关于“如何实现”的**初步想法**，为后续的技术试验阶段提供**待评估的选项**。**此列表不代表最终决定，也不在此阶段明确哪些必须试验。**
        *   **AI 辅助:** AI 可以根据功能需求（如“需要实时通信”、“需要全文搜索”）或行业常见实践，提示可能的技术选项。
    *   *示例 (自酿啤酒社群 APP - 仅列出设想):*
        *   `平台设想: Web App (SPA)。`
        *   `前端框架考虑: Vue 3 + Vite。`
        *   `前端状态管理考虑: Pinia。`
        *   `后端 API 框架考虑: FastAPI (Python)。`
        *   `数据库考虑: PostgreSQL。`
        *   `数据访问考虑: Prisma ORM。`
        *   `认证机制考虑: JWT。`
        *   `部署考虑 (初步): Cloudflare Pages (前端), Docker Compose (后端本地)。`
        *   `关键外部依赖 (MVP): 无。`
        *   `数据结构考虑: 配方的 'ingredients' 字段可能需要灵活结构 (如 JSON)。`
    *   **注释:** 可以简单注释选择某个技术的初步理由（如果明确）。`// FastAPI: Python 生态，异步支持好，文档自动生成。`

6.  **定义初步成功指标:** (同前) 如何判断 MVP 假设被验证（功能层面）。
7.  **明确排除项:** (同前) 清晰列出不做。

## 5. 输出：《MVP 蓝图设计文档.md》

*   一份结构清晰的 Markdown 文档，包含上述所有部分。
*   **特点:**
    *   语言偏向工程实现，功能描述具体且有优先级。
    *   用户流程结合视觉原型。
    *   **清晰列出了高层次的技术设想/考虑点，但不包含强制性的技术试验要求或 Spike 标记。** 它定义了“做什么”和“初步想怎么做”。

## 6. 价值与目的

*   **定义清晰的 MVP 范围与目标:** 为后续所有阶段提供统一的参照物。
*   **结构化工程输入:** 将需求和原型转化为更适合技术团队（包括 AI）理解的格式。
*   **指导技术试验方向:** “高层技术考虑”部分为下一阶段（MVP 技术试验）提供了需要评估和验证的技术选项列表。
*   **作为 UML 设计和 Phase 1 编码的基础:** 明确了要构建的核心功能和流程。

## 7. 结论

AI 时代的 MVP 蓝图设计，是在 AI 辅助下，快速将早期需求和视觉概念**固化为一份清晰、结构化的工程目标文档**的过程。它严格定义了 MVP 的**“What”（做什么）**，并提出了初步的**“How”（打算怎么做）**的技术设想。这份蓝图**不负责验证技术可行性（这是下一阶段的任务）**，而是专注于提供一个**稳定、明确的参照点**，指导后续的技术试验、UML 设计和第一阶段的编码实现，确保整个团队（人与 AI）朝着同一个最小化但清晰的目标前进。