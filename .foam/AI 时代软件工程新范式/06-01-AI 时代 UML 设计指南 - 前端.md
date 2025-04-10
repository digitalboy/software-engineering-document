# AI 时代 UML 设计指南 - 前端.md
**(聚焦界面结构、状态与交互，AI 友好)**

## 1. 引言：为前端自动化构建可视化结构蓝图

本指南是《AI 时代 UML 设计指南.md》（总则）在**前端开发（浏览器 Web 应用及移动端 App）**场景下的具体实践细则。在 AI 协同开发流程中，前端 UML 设计的目标是：

**利用 AI 辅助，基于 MVP 蓝图、视觉高保真原型 (HTML/CSS) 和相关技术试验结论，生成一套层次化、AI 友好的文本 UML (PlantUML/Mermaid) 规约。这份规约需清晰定义前端应用的界面逻辑结构、核心组件关系、状态管理机制、关键用户交互流程以及与后端 API 的通信契约，旨在精确指导下游 AI 工具（如前端代码/测试生成器）和开发者的实现工作。**

**核心原则:** 严格遵循总则，同时**特别强调对界面结构、状态管理和用户交互逻辑的建模，并坚决排除像素级 UI 细节和复杂样式实现**。

## 2. 核心原则回顾 (前端侧重)

*   **AI 优先, 文本 UML:** 为机器解析和生成优化。
*   **分层细化 (L1-L3):** 从应用整体交互 -> 模块/视图划分 -> 内部组件/状态/服务细节。
*   **MECE:** 确保视图、组件、状态存储、服务等职责清晰、无重叠、覆盖核心功能。
*   **基于验证:** 模型需反映技术试验（UI 框架、状态库、API Client）的结论。
*   **聚焦核心结构与行为:** 建模 MVP 范围内的关键视图、**逻辑组件**、**状态**、**用户核心流程**和 **API 交互**。
*   **注释提供上下文:** 解释设计决策、状态逻辑、API 契约、以及指向 Spikes 或 UI 原型的引用。
*   **AI 生成，人工确认:** AI 提效，开发者把关准确性和合理性。
*   **边界明确:** **坚决不使用 UML 描述详细的 UI 布局、CSS 样式、动画、具体控件外观或复杂的 DOM 操作逻辑。** 这些留给编码阶段（开发者 + AI 代码助手 + 视觉原型参考）。

## 3. 输入

*   《MVP 蓝图设计文档.md》 (包含前端相关的功能、流程)
*   **视觉高保真原型 (HTML/CSS):** **关键输入！** 提供界面结构、元素和导航信息。
*   《MVP 技术试验系列文档》 (特别是前端框架、状态管理库、API Client 的 Spikes)
*   (可选) 初步需求文档/用户故事集

## 4. 分层建模流程 (前端视角)

由“AI UML 架构师”工具或开发者与 LLM 交互执行：

**Level 0: 准备与分析**

*   AI 解析输入，重点理解前端应用的目标用户、核心任务、以及从原型中识别出的主要界面和元素。

**Level 1: 应用上下文层 (定义用户与 App、App 与 API 的交互)**

*   **AI 生成 + 开发者审阅:**
    *   **主要图:** 高层用例图 (Actor 是最终用户)、C4 风格上下文图 (展示用户 <-> 前端 App <-> 后端 API)。
    *   **内容:** 明确用户能通过前端完成的核心任务，以及前端作为独立系统与后端 API 的交互边界。
    *   **MECE 检查:** 是否覆盖核心用户目标？与后端 API 的交互是否清晰？

**Level 2: 关键视图/模块层 (划分前端内部结构)**

*   **AI 生成 + 开发者审阅:**
    *   **主要图:** 组件图或包图。
    *   **内容:** 基于原型结构和技术选型（如 Vue/React + Pinia/Redux），将前端应用分解为主要逻辑区域：
        *   **核心视图/页面 (Views/Pages):** 代表主要的用户界面屏幕。
        *   **路由管理 (Router):** 定义视图间的导航逻辑。
        *   **共享逻辑组件 (Shared Components - Logical):** **非 UI 细节**，而是代表可复用的界面区域或逻辑单元（如 `RecipeCardDisplay`, `CommentSection`）。
        *   **状态管理 (State Stores):** 核心的状态容器（如 `AuthStore`, `RecipeStore`）。
        *   **API 服务层 (API Services):** 封装对后端 API 的调用。
        *   **(可选) 工具/助手函数 (Utils/Helpers):** 通用逻辑。
    *   **MECE 检查:** 模块划分是否覆盖 L1 用户任务所需界面和逻辑？职责（视图渲染、状态管理、API 调用）是否清晰分离？关键依赖关系（如 View -> Store -> Service）是否表示？

**Level 3: 内部细节层 (深入视图/组件/状态/服务)**

*   **AI 生成 + 开发者审阅 (针对 L2 的每个关键模块):**
    *   **类图 (主要用于结构和接口):**
        *   **状态存储 (Stores):** **核心！** 定义 State 的**数据结构/形状**，Actions/Mutations/Getters 的**方法签名**及其参数/返回类型 (DTOs)。
        *   **API 服务:** 定义调用后端 API 的**方法签名**，请求/响应的 DTO 类型。
        *   **高层组件结构:** (可选，谨慎使用) 绘制**关键视图**与其包含的**主要逻辑子组件**之间的关系图，展示**数据传递 (Props) 和事件发出 (Events) 的高级契约**。**再次强调：不绘制具体 UI 控件 (Button, Input) 的类。**
            *   *示例: `RecipeListView` 包含多个 `RecipeCardDisplay`，并可能向其传递 `recipeData` prop。`RecipeCardDisplay` 可能发出 `addToFavorites` 事件。*
        *   **(可选) 工具类/函数签名:**
    *   **序列图 (主要用于交互流程):** **核心！**
        *   描绘**关键用户交互**（如按钮点击、表单提交）触发的**前端内部逻辑流**。
        *   清晰展示：`用户操作 -> View/Component -> Store Action -> (State Mutation) -> API Service Call -> (Update State based on Response) -> View Rerender (逻辑层面)`。
        *   **必须反映 Spikes 中验证过的状态管理模式和 API Client 用法。**
        *   **不展示底层的 DOM 事件冒泡或精确的渲染更新细节。**
    *   **状态机图 (可选，用于复杂状态逻辑):**
        *   描述某个**视图或逻辑组件**（非 UI 控件）的**复杂内部状态转换**（例如，一个多步骤的表单向导视图的状态：Step1 -> Step2 -> Submitting -> Success/Error）。
        *   **不用来描述按钮的 `disabled`/`enabled` 或输入框的 `focus`/`blur` 状态。**
    *   **MECE 检查 (组件内):** 是否覆盖了该模块的核心职责（状态定义？API 调用接口？关键流程处理？）？接口签名是否清晰？

## 5. 关键 UML 类型与前端焦点

*   **用例图 (L1):** 用户能用前端完成什么。
*   **组件图/包图 (L2):** 前端代码的宏观架构。
*   **类图 (L3):** **核心在于 State 结构、Action/Service 接口签名、高层组件间的数据/事件契约。**
*   **序列图 (L3):** **核心在于可视化用户交互如何驱动状态变化和 API 通信。**
*   **状态机图 (L3 - 可选):** 用于复杂视图或逻辑组件的内部状态管理。

## 6. 注释的最佳实践 (前端)

*   **状态存储 (Store):** 注释 State 中每个字段的含义，Action 的目的、参数、预期效果（状态变化）和依赖的 API 调用。
*   **API 服务:** 注释每个方法对应的后端 API 端点、请求/响应 DTO 结构、预期的成功/错误场景。
*   **高层组件:** 注释其核心职责、接收的关键 Props 和发出的主要 Events。
*   **序列图:** 注释关键步骤的目的、涉及的状态变化、API 调用的预期结果。
*   **边界注释:** 在 L2 或 L3 的相关图中明确注释 **"Detailed UI layout, styling, and specific control implementation are out of scope for this UML."**
*   **指向原型/Spike:** `// See UI Prototype screen X for visual reference.` `// API call structure based on spike-api-client.md.`

## 7. 输出：给前端 AI 和开发者的蓝图

*   一个层次化的包，包含各层级的 PlantUML/Mermaid 文件和详尽注释。
*   **主要用途:**
    *   驱动 AI 生成前端的**视图/页面骨架代码**。
    *   驱动 AI 生成**状态管理代码 (Pinia/Redux Store Boilerplate)**。
    *   驱动 AI 生成**API 服务层/客户端代码**。
    *   驱动 AI 生成**路由配置框架**。
    *   为开发者提供清晰的**组件结构、状态逻辑和交互流程**参考。
*   **明确不直接用于:** 生成像素完美的 UI 或复杂的 CSS。

## 8. 结论

本前端 UML 设计指南强调利用 AI 和文本 UML，为前端应用构建一个**聚焦于逻辑结构、状态管理和核心交互**的层次化蓝图。通过严格定义边界，将 UI 实现细节留给编码阶段，我们可以创建一个既能有效指导 AI 代码生成，又能为人类开发者提供清晰架构视图的高效规约。这使得前端开发在 AI 时代能够更快地从概念走向可交互的核心骨架。