---
name: unity-testability
description: "Advises on Unity testability — isolating logic out of MonoBehaviour and planning EditMode/PlayMode tests. Use when improving testability, extracting logic for unit tests, or planning a test strategy, even if the user just says \"怎么测试\" or \"代码不好测\". 为 Unity 可测试性提供建议(把逻辑从 MonoBehaviour 中剥离、规划 EditMode/PlayMode 测试);当用户要提升可测试性、为单元测试抽取逻辑、或规划测试策略时使用。 VI: testability: tách logic khỏi MonoBehaviour, unit test, EditMode/PlayMode strategy. Dùng module này khi user nói tiếng Việt về các chủ đề này."
---

# Unity Testability Advisor

## Ghi chú tiếng Việt cho agent

- Khi user nói tiếng Việt như: `testability: tách logic khỏi MonoBehaviour, unit test, EditMode/PlayMode strategy`, ưu tiên đọc module này.
- Giữ nguyên tên skill, tham số, endpoint và JSON shape; chỉ dịch ý định của user sang schema gốc.
- Module này là advisory/design docs, không có REST skill trực tiếp; dùng để định hướng trước khi viết/sửa code Unity.

Use this skill when deciding what logic should remain in Unity-facing classes and what should move into pure C# code.

## Review Questions

- Can the rule/algorithm run without `Transform`, `GameObject`, or scene state?
- Can config be injected instead of read through static globals?
- Can runtime decisions be moved to a plain C# class and called from a thin `MonoBehaviour`?
- Does this need PlayMode coverage, or is EditMode enough?

## Output Format / Định dạng trả lời

- Logic that should move to pure C#
- Logic that should stay Unity-facing
- Suggested seams/interfaces
- Candidate EditMode tests
- Candidate PlayMode tests

## Guardrails / Rào chắn

> **Mode**: Documentation only — no REST skills to gate; load freely under any operating mode (Approval / Auto / Bypass).

- Do not force test seams everywhere if the script is tiny and scene-bound.
- Prefer a few meaningful seams over abstraction for its own sake.
