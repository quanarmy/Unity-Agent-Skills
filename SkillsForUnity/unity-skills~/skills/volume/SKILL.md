---
name: unity-volume
description: "Work with the SRP Volume framework — create/load VolumeProfile assets and create global/local Volume GameObjects with components. Use when setting up volumes, creating or loading a VolumeProfile, or adding global/local volumes to a scene, even if the user just says \"Volume\" or \"体积\". 使用 SRP Volume 框架(创建/加载 VolumeProfile 资产、创建全局/局部 Volume GameObject 及组件);当用户要搭建 Volume、创建或加载 VolumeProfile、或向场景添加全局/局部 Volume 时使用。 VI: Volume/VolumeProfile: post-processing volume, component, parameter override. Dùng module này khi user nói tiếng Việt về các chủ đề này."
---

# Volume Skills

## Ghi chú tiếng Việt cho agent

- Khi user nói tiếng Việt như: `Volume/VolumeProfile: post-processing volume, component, parameter override`, ưu tiên đọc module này.
- Giữ nguyên tên skill, tham số, endpoint và JSON shape; chỉ dịch ý định của user sang schema gốc.
- Trước lần execute đầu, dùng `GET /skills/recommend` hoặc `POST /skill/<name>?mode=dryRun` để xác nhận tham số.
- Với thao tác tạo/sửa/xoá/batch, kiểm tra `Operating Mode`, grant/allowlist/confirmation trước khi chạy thật.
- Nếu tác vụ chạm 2+ object/asset/item, tìm bản `*_batch` trước khi lặp single skill.

Shared SRP Volume framework skills for Unity 2022.3+ — works in URP and HDRP via SRP Core.

## Operating Mode / Chế độ quyền

- Query skills (`volume_list_component_types`, `volume_get_component`) are `SkillMode.SemiAuto` — they run in all three modes without grant.
- Mutating skills (`volume_profile_create`, `volume_create`, `volume_set_profile`, `volume_add_component`, `volume_set_parameter`, `volume_set_parameter_batch`) are `SkillMode.FullAuto` — under **Approval** they need user grant (grant triggers one server-side execute returning the result); under **Auto** / **Bypass** they execute directly.
- `volume_remove_component` carries `SkillOperation.Delete` and is **auto-forbidden** in Approval / Auto modes (NeverInSemi). Only **Bypass** or the user-managed **Allowlist** can run it.

## SRP Package Stub

This module is compiled against `com.unity.render-pipelines.core` (`SRP_CORE`). When neither URP nor HDRP is installed (no SRP Core), **every** skill returns a stub `{ error: "Scriptable Render Pipeline Core package … is not installed." }` (`RenderPipelineSkillsCommon.NoSRP()`). The stub is a diagnostic payload, not a permission denial — it does **not** require grant and is **not** treated as NeverInSemi. Inspect `project_get_render_pipeline` first when you see this error.

## Guardrails / Rào chắn

**Routing / Điều hướng**:
- For Volume container/profile CRUD: use this module
- For high-level modern post-processing effects like Bloom/DOF/Tonemapping: prefer `postprocess`

**Runtime-first rules**:
- Always call `volume_list_component_types` before assuming a component type exists on the active pipeline
- Use `volume_get_component` after add/create to inspect the actual parameter names before writing values
- Prefer exact parameter names returned by the live component data instead of guessing from memory
- `volume_set_parameter_batch` expects `items` to be a JSON array string

## Skills

### `volume_profile_create`
Create a VolumeProfile asset.

### `volume_create`
Create a global or local Volume GameObject.

### `volume_set_profile`
Assign or replace the profile on an existing Volume.

### `volume_list_component_types`
List explicit supported VolumeComponent types for the active SRP pipeline.

### `volume_add_component`
Add a VolumeComponent override to a VolumeProfile.

### `volume_remove_component`
Remove a VolumeComponent override from a VolumeProfile.

### `volume_get_component`
Inspect a VolumeComponent override and its parameters.

### `volume_set_parameter`
Set one override parameter on a VolumeComponent.

### `volume_set_parameter_batch`
Set multiple override parameters on one VolumeComponent.

---
## Exact Signatures

Exact names, parameters, defaults, and returns are defined by `GET /skills/schema` or `unity_skills.get_skill_schema()`, not by this file.
