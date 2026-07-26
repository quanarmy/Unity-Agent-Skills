---
name: unity-scene-contracts
description: "Advises on Unity scene composition contracts — required scene objects, component dependencies, bootstrap logic, and reference wiring. Use when defining what a scene must contain, planning bootstrap/wiring, or documenting scene dependencies, even if the user just says \"场景里要有什么\" or \"引用怎么连\". 为 Unity 场景装配契约提供建议(场景必备对象、组件依赖、bootstrap 逻辑、引用连线);当用户要界定场景必须包含什么、规划启动/装配、或记录场景依赖时使用。 VI: scene contract: object bắt buộc, reference wiring, bootstrap scene, validation. Dùng module này khi user nói tiếng Việt về các chủ đề này."
---

# Unity Scene Contracts

## Ghi chú tiếng Việt cho agent

- Khi user nói tiếng Việt như: `scene contract: object bắt buộc, reference wiring, bootstrap scene, validation`, ưu tiên đọc module này.
- Giữ nguyên tên skill, tham số, endpoint và JSON shape; chỉ dịch ý định của user sang schema gốc.
- Module này là advisory/design docs, không có REST skill trực tiếp; dùng để định hướng trước khi viết/sửa code Unity.

Use this skill when scene setup needs to be explicit instead of relying on hidden runtime lookups.

## Define

- Required root objects
- Required components on each root
- Which references are assigned in Inspector
- Which objects act as bootstrap/installers
- Which objects are runtime-spawned
- Which assumptions should be validated early

## Output Format / Định dạng trả lời

- Scene object contract
- Bootstrap sequence
- Inspector wiring rules
- Validation rules
- Hidden dependency risks

## Guardrails / Rào chắn

> **Mode**: Documentation only — no REST skills to gate; load freely under any operating mode (Approval / Auto / Bypass).

- Prefer explicit scene wiring over chains of runtime `Find`.
- Keep bootstrap objects small and focused.
