# AI 时代软件开发第一阶段编码指南.md
**(Phase 1: MVP 功能骨架构建与验证 - 注释优先版)**

## 定义：“第一阶段 (Phase 1)” - MVP 功能骨架构建与验证

**“第一阶段 (Phase 1)”** 在本指南中特指 AI 协同开发流程中的一个核心编码阶段。其**唯一且明确的目标**是：

**以最快的速度，基于先前已验证的技术栈（来自 MVP 技术试验）和结构化设计（来自 AI UML 架构师），构建出覆盖 MVP (Minimum Viable Product) 范围内所有核心用户场景的“快乐路径”(Happy Path) 的、端到端（UI <-> API <-> DB）可运行的功能骨架。**

**关键特征与边界:**

*   **目标:** 快速实现功能骨架，验证端到端集成可行性。
*   **范围:** 仅覆盖 MVP 定义的核心功能流。
*   **代码质量:** 接受“刚好够用”的实现，**允许编码粗糙**，不追求性能优化、扩展性或代码优雅性。
*   **逻辑完整性:** 只需实现核心成功路径的逻辑，**明确忽略**复杂的错误处理、边缘情况覆盖和完善的输入验证。
*   **测试重点:** 主要通过**端到端 (E2E) 测试（手动为主或自动）验证核心成功路径**是否跑通。单元和集成测试在此阶段为次要或可选。
*   **核心产出:**
    1.  一个端到端基本可运行的 MVP 功能骨架。
    2.  **一套极其详尽、结构化的代码注释系统**，清晰定义接口契约、数据流、依赖关系，并包含大量明确的、指向 Phase 2 的 `TODO` 标记。**在此阶段，注释质量优先于代码质量，“代码 + 注释”共同构成核心文档。**
    3.  (可选) 覆盖快乐路径的 E2E 测试脚本。
*   **非目标:** 交付生产就绪 (Production-Ready) 的代码；实现高健壮性、高可用性或高性能；覆盖所有可能的错误或边缘情况。

**Phase 1 的成功标志是：** 能够快速、可靠地证明 MVP 核心功能流可以在选定的技术栈上端到端地运行起来（仅限快乐路径），并且所有已知的不完善之处都已被清晰地记录（通过高质量注释和 TODOs），为下一阶段 (Phase 2) 的精化工作铺平了道路。

---

## 1. 引言：执行 Phase 1 - AI 加速的骨架构建与文档化

在明确了 **“第一阶段 (Phase 1)”的核心目标——快速构建并验证 MVP 功能骨架**之后，本指南将详细阐述如何在 AI 的协同下高效执行这一阶段。我们采用“纵向切片”策略，并践行**“编码粗糙，注释明晰”**的核心原则，旨在将先前阶段的设计蓝图快速转化为一个端到端可运行的基础。在此阶段，代码与其附带的详尽注释共同构成了驱动后续工作的核心“文档”，为后续的 Phase 2 精化工作留下清晰的路标。

## 2. 核心原则 (Phase 1 侧重)

1.  **速度至上 (Velocity is King):** 快速完成纵向切片，打通核心流程。
2.  **技术验证落地 (Tech Validation Grounding):** 应用 Spikes 的结论，验证实际集成。
3.  **接口契约先行 (Contract First via Comments):** 通过详尽注释定义清晰接口，即使实现简单。
4.  **注释即核心文档 (Comments as Core Documentation):** 清晰、准确、详尽的注释优先于代码优雅性，指导 Phase 2。
5.  **快乐路径优先 (Happy Path First):** 聚焦核心成功场景，忽略错误和边缘情况。
6.  **明确技术债 (Acknowledge & Document Tech Debt):** 通过详细 `TODO` 主动记录不完善之处。
7.  **AI 为主力生成，人为核心整合与验证 (AI Generates, Human Integrates & Validates):** 最大化利用 AI，开发者聚焦连接、调试和注释质量。

## 3. 输入与准备

*   **必需输入:**
    *   《MVP 蓝图设计文档.md》
    *   《AI UML 架构师规约包》
    *   《MVP 技术试验系列文档》
*   **开发环境:** 按 Spike 结论配置完毕。
*   **AI 工具:** 代码助手 (Copilot, Gemini Code Assist等) 和可能的代码生成工具。

## 4. 核心编码步骤 (按纵向切片迭代)

### 步骤 4.1: 定义切片与识别关键接口

*   **开发者主导:** 选择最小化端到端场景，识别其涉及的主要模块交互点和接口契约（基于 UML）。

### 步骤 4.2: AI 生成骨架与接口定义

*   **开发者指令 + AI 执行:** 指令 AI 根据 UML 生成文件结构、类/接口/函数签名、DTO/Entity 定义，并**包含基础的文档注释框架** (如 JSDoc, Python Docstrings 模板)。
*   **开发者审阅:** 快速检查骨架与 UML 定义的一致性（特别是签名和数据结构）。

### 步骤 4.3: 填充最小化“快乐路径”逻辑与【核心】详尽注释

*   **开发者主导 + AI 辅助:** 这是 Phase 1 最核心的步骤。
*   **编码原则 - “刚好够用”:**
    *   只编写跑通成功场景的最少代码。
    *   允许硬编码、Mock/Stub。 `// TODO: Remove hardcoded X.` `// TODO: Replace mock Y.`
    *   忽略输入验证。 `// TODO: Add validation for Z.`
    *   忽略权限检查。 `// TODO:Security - Implement proper authZ.`
    *   最简错误处理 (允许崩溃或返回简单错误)。
*   **注释原则 - “极尽明晰”:**
    *   **类/模块级注释 (必写):** 职责、主要依赖、对外接口、状态说明 (如果适用)、`// TODO: Phase 2 - [整体性待办事项]`。
        ```python
        # --- RecipeService ---
        # Handles core business logic for recipes (CRUD, listing).
        # Depends on: RecipeRepository, UserRepository (for author checks).
        # Provides interface for: RecipeRouter.
        # TODO: Phase 2 - Implement caching strategy for public recipes.
        class RecipeService:
            # ...
        ```
    *   **方法/函数级注释 (必写 - Docstring/JSDoc):**
        *   `@summary`/`@description`: 目的。
        *   `@param`: **详细描述**每个参数 (类型, 含义)。
        *   `@returns`: **详细描述**成功返回值 (类型, 含义) + **Phase 1 简化的错误返回值** + `TODO` 指向 Phase 2 错误处理。
        *   `@throws`/`@raises` (用 TODO 标记): `// TODO:ErrorHandling - Should throw SpecificError when...`
        *   `@dependency`: 调用的关键服务/函数。
        *   `@todo`: **大量明确的、分类的 TODO** (Validation, ErrorHandling, Refactor, Optimize, EdgeCase, Security, Test)。
            ```typescript
            /**
             * Fetches public recipes with basic pagination. Happy path only for Phase 1.
             * @param page - Page number (defaults to 1).
             * @param pageSize - Items per page (defaults to 10).
             * @returns Array of RecipeDto for the requested page on success. Returns empty array on error in Phase 1.
             * @dependency recipeRepository.findPublicPaginated
             * @todo TODO:Validation - Add validation for page/pageSize (must be positive integers).
             * @todo TODO:ErrorHandling - Handle potential DB errors from repository. Return proper error response in Phase 2.
             * @todo TODO:Optimize - Consider adding total count for pagination metadata in Phase 2.
             */
            async getPublicRecipes(page: number = 1, pageSize: number = 10): Promise<RecipeDto[]> {
              console.log(`Fetching public recipes page ${page}, size ${pageSize}`); // Phase 1 logging
              try {
                // Phase 1: Assume repository call works
                // TODO:Refactor - Move mapping logic to a dedicated mapper.
                const entities = await this.recipeRepository.findPublicPaginated(page, pageSize);
                const dtos = entities.map(e => ({ id: e.recipeId, name: e.name, /*...*/ })); // Basic mapping
                return dtos;
              } catch (error) {
                console.error("Error fetching public recipes (Phase 1)", error);
                // TODO:ErrorHandling - Implement proper error handling and logging for Phase 2.
                return []; // Simple error return for Phase 1
              }
            }
            ```
    *   **接口契约注释 (必写 - API 端点):** HTTP 方法、路径、参数结构 (引 DTO)、成功响应 (状态码, 结构引 DTO)、**Phase 1 忽略的错误响应 + TODO**。
        ```python
        # GET /recipes/public
        # Retrieves a paginated list of public recipes.
        # Query Params: page (int, optional, default 1), pageSize (int, optional, default 10)
        # Response (Success): HTTP 200 OK, body: List[RecipeDto]
        # Response (Errors - Phase 2 TODOs):
        #   - 400 Bad Request (if validation fails for query params)
        #   - 500 Internal Server Error (if DB error occurs)
        @router.get("/public", response_model=List[RecipeDto])
        async def list_public_recipes(...):
            # Phase 1: Call service, basic return
            # TODO:Validation - Add query parameter validation using FastAPI dependencies.
            # TODO:ErrorHandling - Add global exception handler for 500 errors.
            dtos = await recipe_service.getPublicRecipes(page, pageSize)
            return dtos
        ```
    *   **数据流/依赖注释:** 在关键逻辑点添加简短说明。
*   **利用 AI:** 指令 AI 生成注释框架，解释代码段，或根据 TODO 描述建议初步的（可能粗糙的）代码修改。

### 步骤 4.4: 本地集成与 E2E 快乐路径测试

*   **开发者主导:** 运行、手动测试核心成功流程。调试连接和基本数据流问题。
*   **(推荐) 自动化 E2E 快乐路径:** 编写或让 AI 生成只覆盖成功场景的 E2E 测试脚本。

### 步骤 4.5: (可选但推荐) 部署切片

*   (同前) 验证部署流程。

### 步骤 4.6: Phase 1 切片收尾 - 注释审查

*   **代码回顾核心: 审查注释！**
    *   注释是否清晰、准确、详尽地反映了意图、接口、依赖和所有已知 TODO？
    *   代码是否能跑通快乐路径？
*   **不做大规模重构，除非是为保证接口清晰或 E2E 联通性。**

## 5. Phase 1 核心产出

*   一个**功能骨架系统**，覆盖 MVP 核心流程的快乐路径，端到端基本可运行。
*   一套**极其详尽、结构化的代码注释系统**，是 Phase 1 最重要的产出之一，构成了 Phase 2 的基础和指南（“代码+注释即文档”）。
*   (可选) 覆盖快乐路径的 E2E 自动化测试脚本。
*   一份明确的、按优先级分类（隐含在注释中或单独整理）的 **Phase 2 工作清单 (TODOs)**。

## 6. 结论

AI 时代的第一阶段编码，是通过**速度优先、注释驱动**的方式，快速构建和验证 MVP 的核心骨架。我们拥抱“编码粗糙”，但要求“注释明晰”，将注释作为连接现在与未来、连接人类与 AI 的关键桥梁。这种策略充分利用 AI 的生成能力，同时通过开发者的引导和验证，确保在高速迭代的同时，为后续的质量提升和系统演进奠定坚实、清晰的基础。Phase 1 的成功，意味着你拥有了一个能跑通核心流程的骨架，以及一份详尽的、指导下一步行动的“蓝图注释”。