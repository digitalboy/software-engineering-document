# “自酿啤酒社群”APP - 前端 UML 样例 (AI UML 架构师输出)

**版本:** 1.0
**日期:** 2025-03-26
**目标读者:** 下游 AI 工具 (代码生成器等), 前端开发者 (作为高级蓝图)
**核心原则:** AI 优先, 文本 UML (PlantUML), 分层细化, MECE, 聚焦核心结构与行为, **明确排除 UI 细节**。

---

## Level 1: 应用上下文层 (Frontend App Context)

**目标:** 定义前端应用与外部世界的交互边界。

### `level_1_context/main_use_cases.puml`

```plantuml
@startuml
title Homebrew Community App - Frontend Main Use Cases (MVP)

left to right direction
actor "Homebrewer" as User
rectangle "Frontend App" as App {
  usecase "Login / Register" as UC_Auth
  usecase "View Own Recipes" as UC_ViewOwn
  usecase "Create/Edit Recipe" as UC_ManageRecipe
  usecase "Delete Recipe" as UC_DeleteRecipe
  usecase "View Own Brew Logs" as UC_ViewLogs
  usecase "Create/Edit Brew Log" as UC_ManageLog
  usecase "Delete Brew Log" as UC_DeleteLog
  usecase "Browse Community Recipes" as UC_Browse
  usecase "View Recipe Details" as UC_ViewDetail
  usecase "Add Comment to Recipe" as UC_AddComment
}

User -- UC_Auth
User -- UC_ViewOwn
User -- UC_ManageRecipe
User -- UC_DeleteRecipe
User -- UC_ViewLogs
User -- UC_ManageLog
User -- UC_DeleteLog
User -- UC_Browse
User -- UC_ViewDetail
User -- UC_AddComment

note as N1
  **Scope:** MVP Features.
  Authentication required for all 'Manage' and 'Own' use cases.
end note
@enduml
```

### `level_1_context/context_diagram_c4.puml`

```plantuml
@startuml
!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Container.puml

LAYOUT_WITH_LEGEND()

Person(user, "Homebrewer", "User accessing the app via browser or mobile device.")
System_Boundary(app_boundary, "Homebrew Community App") {
    Container(frontendApp, "Frontend Application", "Vue 3 / Vite / Pinia", "Provides the user interface, manages local state, interacts with the backend.")
}
System_Ext(backendApi, "Backend API", "Provides data, business logic, and authentication via HTTPS/JSON.")

Rel(user, frontendApp, "Uses", "HTTPS")
Rel(frontendApp, backendApi, "Makes API Calls", "HTTPS/JSON")

@enduml
```

### `level_1_context/annotations_l1.md`

*   **Focus:** Defines *what* the user can do via the frontend and *which systems* the frontend interacts with externally.
*   **Boundary:** The internal workings of the "Backend API" are out of scope for the frontend context.

---

## Level 2: 关键视图/模块层 (Frontend App Components/Modules)

**目标:** 划分前端应用内部的主要逻辑区域及其依赖关系。

### `level_2_components/component_diagram.puml`

```plantuml
@startuml
title Frontend App - Logical Components (MVP)

package "User Interface Layer" {
  [Views/Pages] <<Component>> #LightBlue
  [Shared UI Components (Logical)] <<Component>> #LightCyan
}

package "State Management" {
  [Pinia Stores] <<Component>> #LightGreen
  note right: AuthStore, RecipeStore,\n BrewLogStore, etc.
}

package "Routing" {
  [Vue Router] <<Component>> #Wheat
}

package "Infrastructure/Services" {
  [API Service Wrapper] <<Component>> #LightGray
  note right: Wraps fetch/axios for backend calls
}

' Dependencies
[Views/Pages] ..> [Vue Router] : uses for navigation
[Views/Pages] ..> [Pinia Stores] : reads state, dispatches actions
[Views/Pages] ..> [Shared UI Components (Logical)] : uses
[Shared UI Components (Logical)] ..> [Pinia Stores] : (may read state or dispatch actions)

[Pinia Stores] ..> [API Service Wrapper] : calls backend API via wrapper

[Vue Router] ..> [Views/Pages] : loads views

note as N1
  **Logical Grouping:** This shows logical dependencies, not necessarily folder structure.
  **UI Detail Exclusion:** 'Shared UI Components' represents logical units (like RecipeCard, CommentList), *not* granular elements (Button, Input).
end note
@enduml

```

### `level_2_components/annotations_l2.md`

*   **Architecture:** Standard Single Page Application (SPA) structure with clear separation of concerns (UI, State, Services).
*   **Key Dependencies:** Views depend on State and Router. State depends on API Services.

---

## Level 3: 内部细节层 (View/Component/State/Service Internals)

**目标:** 详细描述 L2 中关键模块的内部结构和核心交互逻辑。

### 组件: `State Management (Pinia)`

#### `level_3_details/state/recipe_store_class.puml`

```plantuml
@startuml
title RecipeStore (Pinia) - Structure

class RecipeStore <<Pinia Store>> {
  + state: RecipeState
  + actions: RecipeActions
  + getters: RecipeGetters (optional)
  ---
  ' Dependencies
  - apiService: ApiService
}

class RecipeState {
  + recipes: Recipe[]
  + isLoadingList: boolean
  + selectedRecipe: Recipe | null
  + isLoadingDetail: boolean
  + error: string | null
}

interface RecipeActions {
  + fetchRecipes(): Promise<void>
  + fetchRecipeById(id: string): Promise<void>
  + createRecipe(data: CreateRecipeDto): Promise<Recipe>
  + updateRecipe(id: string, data: UpdateRecipeDto): Promise<Recipe>
  + deleteRecipe(id: string): Promise<void>
  + addComment(recipeId: string, comment: AddCommentDto): Promise<void>
}

RecipeStore ..> RecipeState
RecipeStore ..> RecipeActions
RecipeStore ..> ApiService : uses

note as N1
  **Focus:** Defines the *structure* of the state and the *signatures* of actions.
  **Implementation Detail:** Specific Pinia setup (`defineStore`) is implementation detail, not shown here.
  **DTOs:** `CreateRecipeDto`, `UpdateRecipeDto`, `AddCommentDto` should align with Backend API DTOs.
end note
@enduml
```

### 组件: `Services`

#### `level_3_details/services/api_service_class.puml`

```plantuml
@startuml
title ApiService - Interface

interface ApiService <<Service Wrapper>> {
  ' Auth '
  + login(credentials: LoginDto): Promise<AuthToken>
  + register(data: RegisterDto): Promise<User>
  ' Recipes '
  + getRecipes(params?: QueryParams): Promise<Recipe[]>
  + getRecipeById(id: string): Promise<Recipe>
  + createRecipe(data: CreateRecipeDto): Promise<Recipe>
  + updateRecipe(id: string, data: UpdateRecipeDto): Promise<Recipe>
  + deleteRecipe(id: string): Promise<void>
  ' Brew Logs '
  + getBrewLogs(recipeId?: string): Promise<BrewLog[]>
  + createBrewLog(data: CreateBrewLogDto): Promise<BrewLog>
  + updateBrewLog(id: string, data: UpdateBrewLogDto): Promise<BrewLog>
  + deleteBrewLog(id: string): Promise<void>
  ' Comments '
  + getComments(recipeId: string): Promise<Comment[]>
  + addComment(recipeId: string, data: AddCommentDto): Promise<Comment>
  ---
  ' Implementation uses fetch or axios internally
  ' Handles base URL, headers (including Auth token), error mapping
}

note as N1
  **Focus:** Defines the *contract* for interacting with the Backend API.
  **DTOs:** Should align with Backend API definitions.
  **Error Handling:** Detailed error handling logic is implementation detail, but interface implies Promises which handle async/errors.
end note
@enduml
```

### 关键用户流程: `Create Recipe`

#### `level_3_details/sequences/sequence_create_recipe.puml`

```plantuml
@startuml
title Sequence Diagram: User Creates a New Recipe

actor User
participant "RecipeCreateView" as View <<Vue Component>>
participant "RecipeStore" as Store <<Pinia Store>>
participant "ApiService" as Service <<Service Wrapper>>
participant "BackendAPI" as Backend <<External System>>

autonumber "<b>[0]"

User -> View: Fills form & Clicks 'Save'
activate View
  View -> Store: dispatch(createRecipe(formData))
  activate Store
    Store -> Store: Set isLoadingList = true (Optimistic UI)
    Store -> Service: createRecipe(formData)
    activate Service
      Service -> Backend: POST /recipes (formData, AuthToken)
      activate Backend
        ' Backend processes request...'
        Backend --> Service: HTTP 201 Created { newRecipe }
      deactivate Backend
      Service --> Store: returns newRecipe
    deactivate Service
    Store -> Store: Add newRecipe to state.recipes
    Store -> Store: Set isLoadingList = false
    Store --> View: Action complete (state updated)
  deactivate Store
  View -> View: Show success message / Navigate away
deactivate View

@enduml
```

#### `level_3_details/annotations_l3_frontend.md` (汇总或分散到具体图注释)

*   **State Management:** Pinia is used for centralized state. Stores encapsulate state logic and API interactions.
*   **API Interaction:** All backend communication goes through the `ApiService` wrapper.
*   **User Flow Focus:** Sequence diagrams illustrate key user interactions driving state changes and API calls.
*   **Structure Focus:** Class diagrams define interfaces and data structures (State, DTOs, Service methods), not visual components.
*   **Boundary Explicit:** Detailed UI layout, CSS, specific form validation rules (beyond data types), and complex animations are explicitly **out of scope** for this UML specification. They should be handled during implementation, potentially guided by UI mockups and real-time AI code assistance.

---

## 输出包结构 (示例)

```
/design_spec_homebrew_frontend_v1.0
    README.md
    /level_1_context
        main_use_cases.puml
        context_diagram_c4.puml
        annotations_l1.md
    /level_2_components
        component_diagram.puml
        annotations_l2.md
    /level_3_details
        /state
            recipe_store_class.puml
            auth_store_class.puml # (Similar structure)
            brew_log_store_class.puml # (Similar structure)
        /services
            api_service_class.puml
        /sequences
            sequence_create_recipe.puml
            sequence_login.puml # (Similar detail level)
            sequence_view_recipe_detail.puml # (Shows fetching data)
        annotations_l3_frontend.md # (Or notes within puml files)
```


