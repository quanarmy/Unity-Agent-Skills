---
name: unity-sample
description: "Sample and demo skills for API connectivity testing — minimal echo/ping-style calls to verify the REST server is reachable. Use when testing whether the UnitySkills REST server responds, smoke-testing connectivity, or trying a first call, even if the user just says \"测试连接\" or \"通不通\". 用于 API 连通性测试的示例/演示 skill(最小的 echo/ping 式调用,验证 REST 服务可达);当用户要测试 UnitySkills REST 服务是否响应、冒烟测试连通性、或试发第一个调用时使用。 VI: sample/demo skills: tạo/xoá object demo, test basic skill surface. Dùng module này khi user nói tiếng Việt về các chủ đề này."
---

# Sample Skills

## Ghi chú tiếng Việt cho agent

- Khi user nói tiếng Việt như: `sample/demo skills: tạo/xoá object demo, test basic skill surface`, ưu tiên đọc module này.
- Giữ nguyên tên skill, tham số, endpoint và JSON shape; chỉ dịch ý định của user sang schema gốc.
- Trước lần execute đầu, dùng `GET /skills/recommend` hoặc `POST /skill/<name>?mode=dryRun` để xác nhận tham số.
- Với thao tác tạo/sửa/xoá/batch, kiểm tra `Operating Mode`, grant/allowlist/confirmation trước khi chạy thật.
- Nếu tác vụ chạm 2+ object/asset/item, tìm bản `*_batch` trước khi lặp single skill.

Basic examples for testing the API.

## Guardrails / Rào chắn

**Operating Mode / Chế độ quyền** (v1.9 three-tier):
- **Approval** (default): query skills (`get_scene_info`, `find_objects_by_name`) run directly. Creators/mutators (`create_cube`, `create_sphere`, `set_object_position`, `set_object_rotation`, `set_object_scale`) are FullAuto — on `MODE_RESTRICTED`, run the grant protocol.
- **Auto** / **Bypass**: SemiAuto and FullAuto run directly.
- Auto-forbidden in this module: `delete_object` (`SkillOperation.Delete`). It is reachable only under Bypass or via a user-managed Allowlist entry; the grant flow returns `MODE_FORBIDDEN`.

**DO NOT / Không gọi nhầm** (common hallucinations):
- Sample skills are basic test/demo skills — do not use them for production work
- `sample_create` is a simplified version of `gameobject_create` — prefer the full gameobject module
- `sample_hello` / `sample_ping` are connectivity test skills only

**Routing / Điều hướng**:
- For actual GameObject operations → use `gameobject` module
- For server health check → use Python helper's `unity_skills.health()`

## Skills

### create_cube
Create a cube primitive.
**Parameters / Tham số:** `x`, `y`, `z`, `name`

### create_sphere
Create a sphere primitive.
**Parameters / Tham số:** `x`, `y`, `z`, `name`

### delete_object
Delete object by name.
**Parameters / Tham số:** `objectName`

### `find_objects_by_name`
Find objects containing string.
**Parameters / Tham số:** `nameContains` (`name` is also accepted as a compatibility alias)

### `set_object_position`
Set object position.
**Parameters / Tham số:** `objectName`, `x`, `y`, `z`

### `set_object_rotation`
Set object rotation.
**Parameters / Tham số:** `objectName`, `x`, `y`, `z`

### `set_object_scale`
Set object scale.
**Parameters / Tham số:** `objectName`, `x`, `y`, `z`

### `get_scene_info`
Get current scene information.
**Parameters / Tham số:** None.

---
## Exact Signatures

Exact names, parameters, defaults, and returns are defined by `GET /skills/schema` or `unity_skills.get_skill_schema()`, not by this file.