---
name: unity-history
description: "Manage undo/redo history over the Unity Editor native undo stack — inspect, step through, and control recorded operations. Use when reviewing or navigating the undo history, stepping undo/redo, or auditing what changed, even if the user just says \"撤销历史\" or \"回退操作\". 管理 Unity 编辑器原生撤销栈上的撤销/重做历史(查看、逐步遍历、控制已记录的操作);当用户要查看或浏览撤销历史、逐步撤销/重做、或审查改动时使用。 VI: history/undo: xem lịch sử, undo/redo task, session history. Dùng module này khi user nói tiếng Việt về các chủ đề này."
---

# History Skills

## Ghi chú tiếng Việt cho agent

- Khi user nói tiếng Việt như: `history/undo: xem lịch sử, undo/redo task, session history`, ưu tiên đọc module này.
- Giữ nguyên tên skill, tham số, endpoint và JSON shape; chỉ dịch ý định của user sang schema gốc.
- Trước lần execute đầu, dùng `GET /skills/recommend` hoặc `POST /skill/<name>?mode=dryRun` để xác nhận tham số.
- Với thao tác tạo/sửa/xoá/batch, kiểm tra `Operating Mode`, grant/allowlist/confirmation trước khi chạy thật.
- Nếu tác vụ chạm 2+ object/asset/item, tìm bản `*_batch` trước khi lặp single skill.

Manage Unity Editor undo/redo history.

## Operating Mode / Chế độ quyền

本模块 `history_get_current`（纯读）标 `SkillMode.SemiAuto`，三档下均可直接执行；`history_undo` / `history_redo` 会改变场景状态，为默认 `SkillMode.FullAuto`（Operation=Execute），Approval 模式下需 grant。**不含 NeverInSemi 高危 skill**。

**DO NOT / Không gọi nhầm** (common hallucinations):
- `history_list` / `history_get` do not exist → use `history_get_current` for current undo group
- `history_clear` does not exist → Unity undo history cannot be cleared via API
- `history_save` does not exist → undo history is managed by Unity automatically

**Routing / Điều hướng**:
- For simple undo/redo → `history_undo` / `history_redo` (this module) or `editor_undo` / `editor_redo`
- For persistent task-level undo → use `workflow` module
- For conversation-level undo → use `workflow` module's `workflow_session_undo`

## Skills

### `history_undo`
Undo the last operation.
**Parameters / Tham số:**
- `steps` (int, optional, default 1): Number of operations to undo.

### `history_redo`
Redo the last undone operation.
**Parameters / Tham số:**
- `steps` (int, optional, default 1): Number of operations to redo.

### `history_get_current`
Get current undo history state.
**Parameters / Tham số:** None.

## Exact Signatures

Exact names, parameters, defaults, and returns are defined by `GET /skills/schema` or `unity_skills.get_skill_schema()`, not by this file.
