---
name: unity-navmesh
description: "Bake and query Unity NavMesh — bake/rebake navigation meshes, query paths, and configure bake settings. Use when setting up navigation, baking a NavMesh, testing agent paths, or tuning bake parameters, even if the user just says \"导航网格\" or \"寻路\". 烘焙与查询 Unity NavMesh(烘焙/重烘焙导航网格、查询路径、配置烘焙设置);当用户要搭建导航、烘焙 NavMesh、测试 agent 路径或调整烘焙参数时使用。 VI: NavMesh/navigation: bake, agent, obstacle, path query, area cost. Dùng module này khi user nói tiếng Việt về các chủ đề này."
---

# NavMesh Skills

## Ghi chú tiếng Việt cho agent

- Khi user nói tiếng Việt như: `NavMesh/navigation: bake, agent, obstacle, path query, area cost`, ưu tiên đọc module này.
- Giữ nguyên tên skill, tham số, endpoint và JSON shape; chỉ dịch ý định của user sang schema gốc.
- Trước lần execute đầu, dùng `GET /skills/recommend` hoặc `POST /skill/<name>?mode=dryRun` để xác nhận tham số.
- Với thao tác tạo/sửa/xoá/batch, kiểm tra `Operating Mode`, grant/allowlist/confirmation trước khi chạy thật.
- Nếu tác vụ chạm 2+ object/asset/item, tìm bản `*_batch` trước khi lặp single skill.

Bake / clear NavMesh data, calculate paths, sample positions, and configure NavMeshAgent / NavMeshObstacle components.

## Operating Mode / Chế độ quyền

- **Approval**：查询类 skill（`navmesh_calculate_path` / `navmesh_sample_position` / `navmesh_get_settings`，源码标 `SkillMode.SemiAuto`）直接执行；变更类（`navmesh_bake` / `navmesh_add_agent` / `navmesh_set_agent` / `navmesh_add_obstacle` / `navmesh_set_obstacle` / `navmesh_set_area_cost`，标 `SkillMode.FullAuto`）需用户 grant，grant 后服务端一步执行返结果。
- **Auto / Bypass**：所有未被禁列表拦截的 skill 直接执行。
- 本模块**含 Delete 类 skill**：`navmesh_clear` 标记为 `SkillOperation.Delete`，会被 `IsForbiddenInSemi` 静态拦截 —— 仅 **Bypass** 模式或将其加入 **Allowlist** 才能调用。
- `navmesh_bake` 同步阻塞主线程；大场景可能很慢，调用前提醒用户。

**DO NOT / Không gọi nhầm** (common hallucinations):
- `navmesh_create` does not exist → use `navmesh_bake` to generate NavMesh
- `navmesh_add_agent_component` / `navmesh_set_agent_speed` do not exist → use `navmesh_add_agent` + `navmesh_set_agent` (convenience wrappers), or `component_add`/`component_set_property` for full control
- NavMesh must be re-baked after scene geometry changes

**Routing / Điều hướng**:
- For NavMeshAgent/NavMeshObstacle components → use `component` module
- For path calculation → `navmesh_calculate_path` (this module)

## Skills

### `navmesh_bake`
Bake the NavMesh (Synchronous). **Warning: Can be slow.**
**Parameters / Tham số:** None.

### `navmesh_clear`
Clear the NavMesh data.
**Parameters / Tham số:** None.

### `navmesh_calculate_path`
Calculate a path between two points.
**Parameters / Tham số:**
- `startX`, `startY`, `startZ` (float): Start position.
- `endX`, `endY`, `endZ` (float): End position.
- `areaMask` (int, optional): NavMesh area mask.

**Returns / Trả về:** `{ status, valid, distance, cornerCount, corners }`

- `status`: NavMeshPathStatus string (`PathComplete` / `PathPartial` / `PathInvalid`)
- `valid`: `true` only when `status == "PathComplete"`
- `distance`: total path length (0 for invalid paths)
- `cornerCount`: number of corner points in `corners`
- `corners`: array of `{x, y, z}` waypoints (empty when no path)

### `navmesh_add_agent`
Add NavMeshAgent component to an object.

| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| name | string | No | null | GameObject name |
| instanceId | int | No | 0 | GameObject instance ID |
| path | string | No | null | GameObject hierarchy path |

**Returns / Trả về:** `{ success, gameObject }`

### `navmesh_set_agent`
Set NavMeshAgent properties (speed, acceleration, radius, height, stoppingDistance).

| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| name | string | No | null | GameObject name |
| instanceId | int | No | 0 | GameObject instance ID |
| path | string | No | null | GameObject hierarchy path |
| speed | float | No | null | Agent movement speed |
| acceleration | float | No | null | Agent acceleration |
| angularSpeed | float | No | null | Agent angular speed |
| radius | float | No | null | Agent radius |
| height | float | No | null | Agent height |
| stoppingDistance | float | No | null | Distance to stop before target |

**Returns / Trả về:** `{ success, gameObject, speed, radius }`

### `navmesh_add_obstacle`
Add NavMeshObstacle component to an object.

| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| name | string | No | null | GameObject name |
| instanceId | int | No | 0 | GameObject instance ID |
| path | string | No | null | GameObject hierarchy path |
| carve | bool | No | true | Enable carving |

**Returns / Trả về:** `{ success, gameObject, carving }`

### `navmesh_set_obstacle`
Set NavMeshObstacle properties (shape, size, carving).

| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| name | string | No | null | GameObject name |
| instanceId | int | No | 0 | GameObject instance ID |
| path | string | No | null | GameObject hierarchy path |
| shape | string | No | null | Obstacle shape (e.g. Box, Capsule) |
| sizeX | float | No | null | Obstacle size X |
| sizeY | float | No | null | Obstacle size Y |
| sizeZ | float | No | null | Obstacle size Z |
| carving | bool | No | null | Enable carving |

**Returns / Trả về:** `{ success, gameObject, shape, carving }`

### `navmesh_sample_position`
Find nearest point on NavMesh.

| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| x | float | Yes | - | Source position X |
| y | float | Yes | - | Source position Y |
| z | float | Yes | - | Source position Z |
| maxDistance | float | No | 10 | Maximum search distance |

**Returns / Trả về:** `{ success, found, point: { x, y, z }, distance }`

### `navmesh_set_area_cost`
Set area traversal cost.

| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| areaIndex | int | Yes | - | NavMesh area index |
| cost | float | Yes | - | Traversal cost value |

**Returns / Trả về:** `{ success, areaIndex, cost }`

### `navmesh_get_settings`
Get NavMesh build settings.

**Parameters / Tham số:** None.

**Returns / Trả về:** `{ success, agentRadius, agentHeight, agentSlope, agentClimb }`

---
## Exact Signatures

Exact names, parameters, defaults, and returns are defined by `GET /skills/schema` or `unity_skills.get_skill_schema()`, not by this file.