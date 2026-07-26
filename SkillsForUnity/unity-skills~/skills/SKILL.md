---
name: unity-skills-index
description: "Index of all Unity Skills modules — functional (REST) modules and advisory (design) modules. Browse available modules, check operating-mode requirements (Approval/Auto/Bypass), and pick the right module for a task. Use when looking for which Unity module handles something, browsing the module catalog, or checking a module's mode requirements, even if the user just says \"有哪些 Unity 技能\" or \"Unity 模块列表\". Unity Skills 所有模块的索引(功能型 REST 模块与建议型设计模块);当用户要查找某事由哪个 Unity 模块处理、浏览模块目录、或确认模块的模式要求时使用。 Vietnamese: Dùng khi user hỏi bằng tiếng Việt như module nào để tạo object, sửa script, xoá asset, chạy test, cài package, debug lỗi, tối ưu project; dùng bảng này để route sang module đúng."
---

# Unity Skills - Module Index

Module docs. Start with [../SKILL.md](../SKILL.md) for mode switching and schema-first rules.

> **Multi-instance**: For version-specific projects, call `unity_skills.set_unity_version(...)` first.
> **Schema-first**: Use `GET /skills/schema` or `unity_skills.get_skill_schema()` for exact signatures. Load module docs for workflow guidance and guardrails.

## Vietnamese Keyword Map / Bản đồ từ khoá tiếng Việt

Dùng bảng này khi user nói tiếng Việt. Sau khi chọn module, vẫn lấy schema bằng `GET /skills/schema?category=<Category>` hoặc `dryRun` để xác nhận tham số.

| Cụm từ tiếng Việt | Module / skill family |
|---|---|
| tạo object, tạo cube, xoá object, đổi tên object | `gameobject` |
| thêm component, xoá component, sửa component | `component` |
| đổi màu, material, shader, texture | `material / shader / shadergraph` |
| ánh sáng, đèn, shadow, light probe | `light` |
| prefab, instantiate, apply override | `prefab` |
| asset, import file, xoá asset, move asset | `asset` |
| script, C#, sửa code, compile lỗi | `script / debug` |
| scene, hierarchy, load scene, save scene | `scene` |
| UI, canvas, button, text, UXML, USS | `ui / uitoolkit` |
| log, console, lỗi đỏ, warning | `console / debug` |
| test, editmode, playmode, chạy kiểm thử | `test` |
| package, UPM, cài Cinemachine | `package` |
| hiệu năng, tối ưu, profiler | `profiler / optimization / performance` |
| kiến trúc, refactor, pattern, testability | `architecture / scriptdesign / patterns / testability` |

## Modules

> **Mode legend** (v1.9.0+, caller-facing — describes what the caller can do, not the C# attribute):
> - `SA` — module skills mostly run directly in **all three modes** (Approval / Auto / Bypass) without a grant.
> - `FA` — module skills mostly require **user grant** under Approval (single-shot one-step execution); under Auto / Bypass they run directly with audit only.
> - `Mixed` — module is split between SA and FA; check per-skill `mode` returned by `GET /skills` before calling.
> - Suffix `*` — module contains auto-forbidden skills (Delete / Play Mode / Domain Reload / high-risk). These return `MODE_FORBIDDEN` under Approval and Auto; only **Bypass** runs them, **or** the user can permanently allow them via the Allowlist. Never attempt grant for them.
>
> Labels are guidance only; the per-skill `mode` field on `GET /skills` is authoritative.

| Module | Mode | Description | Batch Support |
|--------|:----:|-------------|---------------|
| [gameobject](./gameobject/SKILL.md) | FA* | Object create/move/parent — VI: Tạo/sửa/xoá GameObject, transform, parent, layer/tag. | Yes |
| [component](./component/SKILL.md) | Mixed* | Component add/remove/configure — VI: Thêm/xoá/cấu hình Component trên GameObject. | Yes |
| [material](./material/SKILL.md) | FA | Material property edits — VI: Tạo và chỉnh material, màu, shader, texture. | Yes |
| [light](./light/SKILL.md) | FA | Light create/configure — VI: Tạo/chỉnh ánh sáng, shadow, probe, lightmap. | Yes |
| [prefab](./prefab/SKILL.md) | FA | Prefab create/apply/spawn — VI: Tạo prefab, instantiate, apply/revert override, variant. | Yes |
| [asset](./asset/SKILL.md) | SA* | Asset refresh/find/info — VI: Quản lý asset: import, xoá, di chuyển, tìm, label, folder. | Yes |
| [batch](./batch/SKILL.md) | SA | Batch and async jobs — VI: Batch execution, async jobs, progress/report. | Built-in |
| [ui](./ui/SKILL.md) | FA | UGUI Canvas/UI creation — VI: UGUI Canvas, Button, Text, Image, Input, layout. | Yes |
| [uitoolkit](./uitoolkit/SKILL.md) | Mixed* | UXML/USS/UIDocument — VI: UXML/USS/UIDocument/PanelSettings/EditorWindow UI. | No |
| [script](./script/SKILL.md) | SA* | Script create/read/update — VI: Tạo/đọc/sửa C# script, rename/move, compile feedback. | Yes |
| [scene](./scene/SKILL.md) | SA* | Scene load/save/query — VI: Tạo/load/save scene, đọc hierarchy, tìm object. | No |
| [editor](./editor/SKILL.md) | SA* | Play/select/undo/redo/change journal — VI: Điều khiển Unity Editor: play/stop/select/undo/menu/context. | No |
| [animator](./animator/SKILL.md) | FA | Animator controllers — VI: Animator Controller, parameter, state, transition. | No |
| [shader](./shader/SKILL.md) | Mixed* | Shader create/list — VI: Shader code, keyword, list, error check. | No |
| [shadergraph](./shadergraph/SKILL.md) | Mixed* | Shader Graph create/inspect/blackboard edit/constrained node editing — VI: Shader Graph, blackboard, property, node editing. | No |
| [graphics](./graphics/SKILL.md) | Mixed | GraphicsSettings / QualitySettings / SRP assets — VI: Graphics/Quality/SRP settings. | No |
| [volume](./volume/SKILL.md) | Mixed* | Volume / VolumeProfile / VolumeComponent — VI: VolumeProfile, VolumeComponent, override parameter. | No |
| [postprocess](./postprocess/SKILL.md) | FA* | Modern URP/HDRP post-processing — VI: URP/HDRP post-processing effects. | No |
| [urp](./urp/SKILL.md) | Mixed* | URP asset / renderer / renderer features — VI: URP asset, renderer, renderer feature. | No |
| [decal](./decal/SKILL.md) | Mixed* | URP Decal Projector workflow — VI: URP Decal Projector và renderer feature. | Yes |
| [console](./console/SKILL.md) | SA | Log capture/filter — VI: Đọc/lọc/xuất Unity Console logs. | No |
| [validation](./validation/SKILL.md) | SA* | Broken reference checks — VI: Kiểm tra missing script, broken reference, cleanup. | No |
| [importer](./importer/SKILL.md) | Mixed | Texture/audio/model import — VI: Texture/audio/model import settings. | Yes |
| [cinemachine](./cinemachine/SKILL.md) | FA* | VCam operations — VI: Virtual camera, follow/lookAt, blend/path. | No |
| [probuilder](./probuilder/SKILL.md) | FA* | ProBuilder mesh edits — VI: ProBuilder mesh/face/edge/UV operations. | No |
| [xr](./xr/SKILL.md) | FA | XRI setup — VI: XR Interaction Toolkit rig/controller/interactor. | No |
| [terrain](./terrain/SKILL.md) | FA | Terrain create/paint — VI: Terrain create/paint/detail/tree. | No |
| [physics](./physics/SKILL.md) | Mixed | Raycast/overlap/gravity — VI: Physics query, collider, rigidbody, gravity. | No |
| [navmesh](./navmesh/SKILL.md) | Mixed* | NavMesh bake/query — VI: NavMesh bake/query/agent/obstacle. | No |
| [timeline](./timeline/SKILL.md) | FA* | Timeline tracks/clips — VI: Timeline track/clip/binding/playback. | No |
| [workflow](./workflow/SKILL.md) | SA* | Task snapshots/undo, batch orchestration — VI: Task snapshot, undo/redo/rollback/plan. | No |
| [cleaner](./cleaner/SKILL.md) | SA* | Unused/duplicate assets — VI: Dọn asset trùng/không dùng/folder rỗng. | No |
| [smart](./smart/SKILL.md) | FA* | Query/layout/auto-bind — VI: Query/layout/auto-bind/snap/randomize thông minh. | No |
| [perception](./perception/SKILL.md) | SA | Scene/project analysis — VI: Phân tích scene/project/context/dependency. | No |
| [camera](./camera/SKILL.md) | FA | Scene View camera — VI: Scene View/camera positioning/capture. | No |
| [event](./event/SKILL.md) | Mixed* | UnityEvent wiring — VI: UnityEvent listener wiring/copy/remove. | No |
| [package](./package/SKILL.md) | Mixed* | UPM install/query — VI: UPM install/remove/list/search/version/dependency. | No |
| [project](./project/SKILL.md) | SA | Project info/settings — VI: Thông tin project/build/player settings read-only. | No |
| [profiler](./profiler/SKILL.md) | SA | Perf statistics — VI: Thông số profiler/performance/memory. | No |
| [optimization](./optimization/SKILL.md) | Mixed | Asset optimization — VI: Tối ưu texture/audio/static/material/LOD. | No |
| [sample](./sample/SKILL.md) | Mixed* | Demo/test skills — VI: Demo/test basic skill surface. | No |
| [debug](./debug/SKILL.md) | SA | Compile/system diagnostics — VI: Compile errors, defines, recompile, stack/system info. | No |
| [test](./test/SKILL.md) | Mixed | Unity Test Runner — VI: Unity Test Runner, discover/run/poll jobs. | No |
| [bookmark](./bookmark/SKILL.md) | SA | Scene View bookmarks — VI: Scene View bookmarks. | No |
| [history](./history/SKILL.md) | SA | Undo/redo history — VI: History/undo/redo session tasks. | No |
| [scriptableobject](./scriptableobject/SKILL.md) | Mixed* | ScriptableObject assets — VI: ScriptableObject create/read/write/JSON/serialized property. | No |
| [netcode](./netcode/SKILL.md) | Mixed* | Netcode for GameObjects setup, prefabs, lifecycle, host/server/client — VI: Netcode for GameObjects setup/RPC/spawn/NetworkVariable. | Yes |
| [yooasset](./yooasset/SKILL.md) | Mixed* | YooAsset hot-update: build bundles, Collector CRUD, BuildReport asset/dependency analysis, PlayMode runtime validation, Reporter/Debugger/AssetArtScanner tools — VI: YooAsset hot-update build/collector/report/runtime validate. | Yes |
| [dotween](./dotween/SKILL.md) | Mixed* | DOTween Pro DOTweenAnimation editor-time configuration (add/batch/stagger/tune) — VI: DOTween animation/tween/sequence editor setup. | Yes |
| [primetween](./primetween/SKILL.md) | Mixed* | PrimeTween Free inspection, factory discovery, and runtime tween/sequence script generation — VI: PrimeTween tween/sequence inspect và sinh script runtime. | No |

## Advisory Design Modules

These modules provide design guidance only.

| Module | Description |
|--------|-------------|
| [project-scout](./project-scout/SKILL.md) | Inspect existing project — VI: khảo sát project: Unity version, packages, asmdef, folder structure, coding patterns trước khi sửa |
| [architecture](./architecture/SKILL.md) | Plan system boundaries — VI: kiến trúc Unity: module boundary, decoupling, bootstrap, SOLID, hướng refactor |
| [adr](./adr/SKILL.md) | Record tradeoffs — VI: ADR/quyết định kỹ thuật: so sánh lựa chọn, tradeoff, ghi lại quyết định |
| [performance](./performance/SKILL.md) | Review hot paths — VI: review hiệu năng: hot path, Update, allocation, pooling, physics, render cost |
| [asmdef](./asmdef/SKILL.md) | Plan asmdef deps — VI: asmdef: tách assembly, dependency, editor/runtime/test split, compile time |
| [blueprints](./blueprints/SKILL.md) | Small-game blueprints — VI: blueprint game nhỏ: platformer, shooter, runner, puzzle, tower-defense, card, clicker |
| [script-roles](./script-roles/SKILL.md) | Assign class roles — VI: vai trò class: MonoBehaviour, ScriptableObject, plain C# service, installer/bootstrap |
| [scene-contracts](./scene-contracts/SKILL.md) | Define scene wiring — VI: scene contract: object bắt buộc, reference wiring, bootstrap scene, validation |
| [testability](./testability/SKILL.md) | Extract testable logic — VI: testability: tách logic khỏi MonoBehaviour, unit test, EditMode/PlayMode strategy |
| [patterns](./patterns/SKILL.md) | Choose patterns — VI: pattern Unity: event, state machine, object pool, observer, ScriptableObject pattern |
| [async](./async/SKILL.md) | Choose async model — VI: async Unity: coroutine, Update loop, UniTask, cancellation, lifetime cleanup |
| [inspector](./inspector/SKILL.md) | Design authoring UX — VI: Inspector UX: SerializeField, Tooltip, Header, validation, custom editor authoring |
| [scriptdesign](./scriptdesign/SKILL.md) | Review script structure — VI: thiết kế script: coupling, responsibility, dependency, maintainability, code review |
| [netcode-design](./netcode-design/SKILL.md) | Netcode source-anchored rules (lifecycle/ownership/RPC/variables/spawn/scene/transport/pitfalls) — VI: thiết kế Netcode: lifecycle, ownership, RPC, NetworkVariable, spawn, scene, transport |
| [yooasset-design](./yooasset-design/SKILL.md) | YooAsset v2.3.18 source-anchored rules (init/default-package shortcuts/playmode/handles/loading/update/filesystem/build/pitfalls) — VI: thiết kế YooAsset: init, package, playmode, handle, loading, update, filesystem, build |
| [addressables-design](./addressables-design/SKILL.md) | Addressables dual-version (1.22.3 Unity 2022 / 2.9.1 Unity 6) source-anchored rules (init/handles/loading/scene/update/download/assetref/pitfalls) with migration table — VI: thiết kế Addressables: init, handle, load asset/scene, catalog update, download, AssetReference |
| [unitask-design](./unitask-design/SKILL.md) | UniTask 2.5.10 source-anchored rules (basics/playerloop/cancellation/composition/conversion/asyncenumerable/triggers/pitfalls) — VI: thiết kế UniTask: PlayerLoopTiming, CancellationToken, WhenAll/WhenAny, async enumerable |
| [dotween-design](./dotween-design/SKILL.md) | DOTween 1.3.015 source-anchored rules (basics/tween/sequence/shortcuts/ease/lifetime/integration/pitfalls) — VI: thiết kế DOTween: tween, sequence, ease, lifetime, SetLink, UniTask integration |
| [primetween-design](./primetween-design/SKILL.md) | PrimeTween 1.4.6 source-anchored rules (factories/handles/sequences/cycles/callbacks/lifetime/integration) — VI: thiết kế PrimeTween: factory, handle, sequence, cycles, callback, lifetime |
| [shadergraph-design](./shadergraph-design/SKILL.md) | ShaderGraph dual-version source-anchored rules (versions/node subset/recipes/pitfalls/review) — VI: thiết kế Shader Graph: node chain, blackboard, SubGraph, keyword, version pitfalls |
| [yaml-editing](./yaml-editing/SKILL.md) | Safe hand-edit rules for serialized YAML (.unity/.prefab/.asset/.meta/ProjectSettings) when REST cannot reach — reference/fileID repair, .meta/GUID safety, ProjectSettings patch, merge conflict — VI: sửa YAML Unity: .unity/.prefab/.asset/.meta/ProjectSettings, GUID/fileID, merge conflict |

## Batch-First Rule / Quy tắc ưu tiên batch

When a task touches `2+` objects in Auto / Bypass mode (or after a successful grant under Approval), prefer `*_batch` skills over repeated single-item calls.

## Skill Naming Convention / Quy ước tên skill

Skills follow `<module>_<action>` or `<module>_<action>_batch`.
Use schema to verify the exact prefix list.
Special: `scene_analyze`, `hierarchy_describe`, `project_stack_detect` → `perception`; `job_*` → `batch`.
If a skill name does not match a valid prefix or a schema result, do not invent it.
