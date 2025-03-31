# “自酿啤酒社群”APP - 后端 UML 样例 (AI UML 架构师输出)

**版本:** 1.0
**日期:** 2025-03-26
**目标读者:** 下游 AI 工具 (代码生成器, API 文档生成器等), 后端开发者 (作为核心蓝图)
**核心原则:** AI 优先, 文本 UML (PlantUML), 分层细化, MECE, 聚焦核心结构与行为, **明确排除基础设施和底层细节**。

---

## Level 1: 系统上下文层 (Backend API Context)

**目标:** 定义后端 API 与外部世界的交互边界和主要依赖。

### `level_1_context/context_diagram_c4.puml`

```plantuml
@startuml
!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Container.puml

LAYOUT_WITH_LEGEND()

title Homebrew Community App - Backend API Context

System_Ext(frontendApp, "Frontend Application", "Consumes the API via HTTPS/JSON.")
' Potentially other consumers like Mobile App or Admin Tool in the future
' System_Ext(mobileApp, "Mobile App", "...")

System_Boundary(backend_boundary, "Homebrew Community Backend") {
    Container(backendApi, "Backend API", "FastAPI / Python", "Handles business logic, data access, auth for the Homebrew Community App.")
}

SystemDb_Ext(database, "Database", "PostgreSQL", "Stores user data, recipes, logs, comments.")
' Potentially other external systems like an Email Service or a future Payment Gateway
' System_Ext(emailService, "Email Service", "Used for notifications (future).")

Rel(frontendApp, backendApi, "Makes API Calls", "HTTPS/JSON")
Rel(backendApi, database, "Reads/Writes data", "SQL/TCP (via ORM)")
' Rel(backendApi, emailService, "Sends Emails", "HTTPS/API") ' Example for future extension

@enduml
```

### `level_1_context/main_api_capabilities.puml` (可以用 Use Case 图或简单列表)

```plantuml
@startuml
title Backend API - Main Capabilities (MVP)

rectangle "Backend API" as API {
  usecase "Authenticate User (JWT)" as UC_Auth
  usecase "Manage User Profile (Basic)" as UC_User
  usecase "Manage Recipes (CRUD)" as UC_Recipe
  usecase "Manage Brew Logs (CRUD)" as UC_Log
  usecase "Manage Comments (CRD - Create, Read, Delete)" as UC_Comment
  usecase "Provide Public Recipe Feed" as UC_Feed
}

actor "Frontend App" as Client
' Other clients could be added

Client -- UC_Auth
Client -- UC_User
Client -- UC_Recipe
Client -- UC_Log
Client -- UC_Comment
Client -- UC_Feed

note as N1
  **MVP Scope:** Covers core CRUD for main entities and authentication.
  **Authorization:** Access control (e.g., user can only edit own recipes) is implemented within each 'Manage' capability.
end note
@enduml
```

### `level_1_context/annotations_l1.md`

*   **Focus:** Defines *who* consumes the API and *what external systems* the API depends on. Outlines the main functional areas provided.
*   **Boundary:** Internal implementation details are hidden. Frontend interaction details are out of scope for the backend context.

---

## Level 2: 关键模块/服务层 (Backend API Components/Modules)

**目标:** 划分后端 API 内部的主要逻辑分区及其依赖关系。

### `level_2_components/component_diagram.puml`

```plantuml
@startuml
title Backend API - Internal Components/Layers (MVP)

package "API Layer (FastAPI Routers)" {
  [AuthRouter] <<Component>> #LightBlue
  [UserRouter] <<Component>>
  [RecipeRouter] <<Component>>
  [BrewLogRouter] <<Component>>
  [CommentRouter] <<Component>>
  note right: Handles HTTP requests, validation (Pydantic), calls Services
}

package "Business Logic Layer (Services)" {
  [AuthService] <<Component>> #LightGreen
  [UserService] <<Component>>
  [RecipeService] <<Component>>
  [BrewLogService] <<Component>>
  [CommentService] <<Component>>
  note right: Implements core business rules, orchestrates data access
}

package "Data Access Layer (Repositories)" {
  [UserRepository] <<Component>> #Wheat
  [RecipeRepository] <<Component>>
  [BrewLogRepository] <<Component>>
  [CommentRepository] <<Component>>
  note right: Abstracts database interactions (using Prisma ORM)
}

package "Domain Models" {
  [Entities] <<Component>> #LightGray
  note right: User, Recipe, BrewLog, Comment classes/objects
}

' Dependencies
[API Layer (FastAPI Routers)] ..> [Business Logic Layer (Services)]
[Business Logic Layer (Services)] ..> [Data Access Layer (Repositories)]
[Business Logic Layer (Services)] ..> [Domain Models]
[Data Access Layer (Repositories)] ..> [Domain Models]

note as N1
 **Layered Architecture:** Clear separation between API handling, business logic, and data access.
 **Technology Mapping:** Routers map to FastAPI, Repositories use Prisma (from Spikes).
end note
@enduml
```

### `level_2_components/annotations_l2.md`

*   **Architecture:** Layered architecture chosen for maintainability and testability.
*   **Key Dependencies:** API depends on Services, Services depend on Repositories and Domain Models. Direct access from API to Repositories is generally avoided.

---

## Level 3: 内部细节层 (Module/Service Internals)

**目标:** 详细描述 L2 中关键模块/服务的内部结构和核心交互逻辑。

### 领域模型: `Domain Models`

#### `level_3_details/domain/classes_core_models.puml` (已在前端部分展示过，这里可能更详细)

```plantuml
@startuml
title Core Domain Models (Entities)

entity User {
  + userId: UUID <<PK>>
  + email: String <<Unique>>
  + hashedPassword: String
  + createdAt: DateTime
  + updatedAt: DateTime
  --
  + recipes: Recipe[*]
  + brewLogs: BrewLog[*]
  + comments: Comment[*]
}

entity Recipe {
  + recipeId: UUID <<PK>>
  + name: String
  + style: String
  + isPublic: Boolean = false
  + ingredients: JSONB ' Flexible structure for ingredients list
  + instructions: Text
  + authorId: UUID <<FK>>
  + createdAt: DateTime
  + updatedAt: DateTime
  --
  + author: User <<1>>
  + brewLogs: BrewLog[*]
  + comments: Comment[*]
}

entity BrewLog {
  + logId: UUID <<PK>>
  + brewDate: Date
  + notes: Text
  + specificGravityStart: Float?
  + specificGravityEnd: Float?
  + rating: Int? ' (Maybe 1-5)
  + recipeId: UUID <<FK>> ' Log is for a specific recipe instance
  + userId: UUID <<FK>> ' Log belongs to a user
  + createdAt: DateTime
  + updatedAt: DateTime
  --
  + recipe: Recipe <<1>>
  + user: User <<1>>
}

entity Comment {
  + commentId: UUID <<PK>>
  + text: String
  + recipeId: UUID <<FK>>
  + authorId: UUID <<FK>>
  + createdAt: DateTime
  + updatedAt: DateTime
  --
  + recipe: Recipe <<1>>
  + author: User <<1>>
}

User "1" -- "*" Recipe : "authors >"
User "1" -- "*" BrewLog : "logs >"
User "1" -- "*" Comment : "authors >"
Recipe "1" -- "*" BrewLog : "< logs for recipe"
Recipe "1" -- "*" Comment : "< comments on recipe"

note as N1
  **Source of Truth:** These entities represent the core business concepts.
  **ORM Mapping:** These map to database tables via Prisma ORM (from Spikes).
  **Types:** UUID for PKs, JSONB/Text for flexible fields, Nullable types indicated.
end note
@enduml```

### 数据传输对象: `DTOs` (用于 API)

#### `level_3_details/dtos/recipe_dtos.puml`

```plantuml
@startuml
title Recipe API Data Transfer Objects (DTOs)

class RecipeDto {
  + recipeId: UUID
  + name: String
  + style: String
  + isPublic: Boolean
  + ingredients: Object ' Representing parsed JSONB
  + instructions: String
  + authorId: UUID
  + createdAt: DateTime
  + updatedAt: DateTime
  ' อาจจะไม่รวมข้อมูล author ทั้งหมดเพื่อลดขนาด payload
  + authorUsername: String? ' Example: อาจจะเพิ่มข้อมูลบางอย่างของ author
}

class CreateRecipeDto {
  + name: String <<required>>
  + style: String <<required>>
  + isPublic: Boolean = false
  + ingredients: Object <<required>>
  + instructions: String <<required>>
}

class UpdateRecipeDto {
  + name: String?
  + style: String?
  + isPublic: Boolean?
  + ingredients: Object?
  + instructions: String?
}

note as N1
  **Purpose:** Define data shapes for API requests and responses. Decoupled from internal Domain Entities.
  **Validation:** These DTOs are typically used with FastAPI/Pydantic for automatic request validation.
  **Mapping:** Services layer is responsible for mapping between DTOs and Domain Entities.
end note
@enduml
```

### 业务逻辑层: `Services`

#### `level_3_details/services/recipe_service_interface.puml` (展示接口/主要方法签名)

```plantuml
@startuml
title RecipeService - Interface

interface RecipeService <<Service>> {
  + getPublicRecipes(page: int, pageSize: int): Promise<RecipeDto[]>
  + getUserRecipes(userId: UUID, page: int, pageSize: int): Promise<RecipeDto[]>
  + getRecipeById(recipeId: UUID, requestingUserId?: UUID): Promise<RecipeDto | null> ' Check ownership/public status
  + createRecipe(data: CreateRecipeDto, authorId: UUID): Promise<RecipeDto>
  + updateRecipe(recipeId: UUID, data: UpdateRecipeDto, userId: UUID): Promise<RecipeDto | null> ' Check ownership
  + deleteRecipe(recipeId: UUID, userId: UUID): Promise<boolean> ' Check ownership
}

note as N1
  **Responsibilities:** Encapsulates business logic related to recipes (validation, authorization checks, orchestration).
  **Dependencies:** Uses RecipeRepository, potentially UserRepository (for author info). Maps Entities to DTOs.
  **Authorization:** Methods often include `userId` parameter to enforce ownership rules.
end note
@enduml
```

### 关键 API 流程: `Create Recipe`

#### `level_3_details/sequences/sequence_api_create_recipe.puml` (与前端序列图的后端部分对应并细化)

```plantuml
@startuml
title Sequence Diagram: POST /recipes - Create Recipe

participant "RecipeRouter" as Router <<FastAPI Router>>
participant "RecipeService" as Service <<Service>>
participant "RecipeRepository" as Repo <<Repository>>
database "Database" as DB <<PostgreSQL>>

autonumber "<b>[B0]" ' B for Backend

Router -> Router: Validate Request Body (Pydantic DTO: CreateRecipeDto)
alt Validation Fails
  Router --> Client: HTTP 422 Unprocessable Entity
else Validation Success
  Router -> Router: Get authenticated userId from JWT
  Router -> Service: createRecipe(validatedData, userId)
  activate Service
    ' Business logic/validation within Service (e.g., check name uniqueness?)
    Service -> Repo: save(recipeEntity) ' recipeEntity created from DTO + authorId
    activate Repo
      Repo -> DB: INSERT INTO "Recipes" (...) VALUES (...)
      activate DB
      DB --> Repo: Success (returns saved record)
      deactivate DB
      Repo --> Service: Saved Recipe Entity
    deactivate Repo
    Service -> Service: Map Entity to RecipeDto
    Service --> Router: RecipeDto
  deactivate Service
  Router --> Client: HTTP 201 Created { recipeDto }
end

@enduml
```

### 核心实体状态: `Recipe` (如果状态复杂)

#### `level_3_details/state_machines/recipe_state.puml` (此例中 Recipe 状态简单，可能不需要)

```plantuml
@startuml
title Recipe State Machine (Illustrative - MVP state is simple)

[*] --> Private : On Create (isPublic=false)
Private --> Public : On Publish Action
Public --> Private : On Unpublish Action
Private --> [*] : On Delete
Public --> [*] : On Delete

note as N1
  For MVP, recipe state (Public/Private) is simple.
  A more complex entity like 'Order' would have states:
  Created -> PendingPayment -> Paid -> Shipped -> Delivered / Cancelled
end note

@enduml
```

### `level_3_details/annotations_l3_backend.md` (汇总或分散)

*   **Domain Model:** Core entities defined, reflecting business concepts.
*   **DTOs:** Separate objects for API contract, validated by framework.
*   **Layering:** Clear separation of Router (API), Service (Logic), Repository (Data).
*   **Data Access:** Prisma ORM used via Repository pattern (based on Spikes).
*   **Authentication:** JWT based, userId extracted for authorization checks in Service layer.
*   **Boundary:** Excludes detailed SQL, infra config, complex algorithm internals. Error handling shown conceptually in sequences.

---

## 输出包结构 (示例)

```
/design_spec_homebrew_backend_v1.0
    README.md
    /level_1_context
        context_diagram_c4.puml
        main_api_capabilities.puml
        annotations_l1.md
    /level_2_components
        component_diagram.puml
        annotations_l2.md
    /level_3_details
        /domain
            classes_core_models.puml
        /dtos
            recipe_dtos.puml
            auth_dtos.puml
            # ... other DTOs
        /services
            recipe_service_interface.puml
            auth_service_interface.puml
            # ... other service interfaces
        /repositories # (Interfaces mainly, implementation implied by ORM choice)
            recipe_repository_interface.puml
            user_repository_interface.puml
            # ... other repository interfaces
        /sequences
            sequence_api_create_recipe.puml
            sequence_api_user_login.puml
            # ... other key API flows
        /state_machines # (If applicable)
            # recipe_state.puml (If needed)
        annotations_l3_backend.md
```

