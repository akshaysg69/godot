# Review Policies

## Rendering and GPU synchronization critical paths
- **Paths**: `servers/rendering/**`, `drivers/vulkan/**`, `drivers/d3d12/**`, `drivers/metal/**`, `drivers/gles3/**`, `servers/rendering/**/*.glsl`
- **Severity**: critical
- **Reason**: Small synchronization/shader/render-path changes can cause deadlocks, corruption, or visual regressions that pass CI but fail on specific hardware/drivers.

## GDExtension and core object ABI surface
- **Paths**: `core/extension/**`, `modules/gdextension/**`, `core/object/**`, `core/variant/**`, `core/extension/gdextension_interface.json`, `core/extension/gdextension_interface.cpp`
- **Severity**: critical
- **Reason**: ABI/lifetime/instancing semantic changes can break third-party bindings and plugins in ways in-repo tests do not cover.

## Platform runtime, windowing, and OS integration
- **Paths**: `platform/windows/**`, `platform/macos/**`, `platform/ios/**`, `platform/linuxbsd/**`, `drivers/apple_embedded/**`, `drivers/unix/**`
- **Severity**: high
- **Reason**: OS-specific event, DPI, lifecycle, and window behavior regressions require real platform validation beyond static checks.

## Android runtime/export/JNI integration
- **Paths**: `platform/android/**`, `platform/android/java/**`, `platform/android/api/jni_singleton.*`, `platform/android/export/**`, `editor/export/**`
- **Severity**: critical
- **Reason**: Lifecycle/plugin/export changes can silently break startup and callbacks and typically require on-device validation.

## iOS lifecycle and plugin forwarding
- **Paths**: `platform/ios/**`
- **Severity**: high
- **Reason**: Delegate/lifecycle forwarding regressions can drop deep-link and activity callbacks despite passing CI.

## Build system, detection, and packaging
- **Paths**: `SConstruct`, `methods.py`, `platform/*/detect.py`, `platform/**`, `main/**`, `modules/**/detect.py`, `SCsub`, `*.py`
- **Severity**: high
- **Reason**: Toolchain/platform build logic changes may fail only on specific environments not covered by PR CI.

## Editor state, layout, and live sync
- **Paths**: `editor/editor_data.cpp`, `editor/editor_node.cpp`, `editor/scene/**`, `editor/docks/**`, `editor/gui/**`, `editor/inspector/**`, `scene/gui/**`
- **Severity**: high
- **Reason**: Interactive editor state/layout/live-edit regressions are often workflow-specific and need manual UX testing.

## Physics and GUI layout sensitive semantics
- **Paths**: `scene/3d/physics/**`, `modules/jolt_physics/**`, `modules/godot_physics_3d/**`, `servers/physics_3d/**`, `scene/gui/**/box_container*`, `scene/gui/**/split_container*`, `scene/gui/**/control*`
- **Severity**: high
- **Reason**: Coordinate-space and sizing logic changes can alter behavior subtly and are hard to validate with automated tests alone.

## OpenXR runtime negotiation and tracking
- **Paths**: `modules/openxr/**`, `scene/3d/xr/**`, `modules/mobile_vr/**`
- **Severity**: critical
- **Reason**: Runtime/extension fallback and tracking lifecycle changes can fail only on specific devices/runtimes.

## Resource loading, parsing, and threaded shutdown
- **Paths**: `core/io/resource_format_binary.cpp`, `scene/resources/resource_format_text.cpp`, `modules/gdscript/**`, `core/io/resource_loader*`, `core/io/**`
- **Severity**: high
- **Reason**: Loading/parsing/thread-shutdown edits can introduce deadlocks, correctness, or scale regressions not reproduced in small tests.

## Export pipeline and encryption behavior
- **Paths**: `editor/export/**`, `core/core_builders.py`, `platform/macos/export/**`, `platform/web/export/**`
- **Severity**: high
- **Reason**: Packaging/encryption/notifier flow regressions may only appear in real export workflows and can produce broken builds.

## Audio threading and reconnect paths
- **Paths**: `drivers/pulseaudio/**`, `servers/audio/**`
- **Severity**: high
- **Reason**: Realtime audio thread/reconnect changes can deadlock or stall under production timing conditions.

## Third-party vendored code
- **Paths**: `thirdparty/**`
- **Severity**: high
- **Reason**: Vendored dependency edits carry licensing, security, and divergence risks needing human provenance review.

## Class reference documentation impact
- **Paths**: `doc/classes/**`, `modules/*/doc_classes/**`
- **Severity**: medium
- **Reason**: Large doc edits can alter API expectations or create translation churn that automated checks cannot judge for quality.

## CI workflow and custom actions changes
- **Paths**: `.github/workflows/**`, `.github/actions/**`
- **Severity**: high
- **Reason**: Workflow changes can silently reduce coverage or alter release artifact behavior despite green checks.

## Instructions
- If a change adds, removes, or weakens defensive guards/fallbacks (including early returns), a human must judge whether the reliability and safety tradeoff is acceptable.
- If default runtime/editor/export behavior or precedence changes, a human must assess migration and backward-compatibility impact on existing projects.
- If a PR adds new core/scripting parameters, knobs, or generalized helpers, a human must decide whether long-term maintenance cost is justified.
- If a PR introduces caching, lazy init, or low-level optimization, a human must verify measurable benefit justifies added complexity.
- If sync/deferred execution model or thread ownership assumptions change, a human must validate ordering and thread-safety semantics.
- If interaction semantics, discoverability, wording, or major editor UX behavior changes, a human must confirm the tradeoff is intentional and acceptable.
- If body-space vs constraint-space semantics or naming change in physics APIs, a human must verify user-facing consistency and clarity.
- If rendering algorithms or temporal behavior change, a human must validate side-by-side visual results across relevant settings/platforms.
- If XR runtime fallback/extension/tracking behavior changes, a human must verify acceptable cross-device degradation and correctness.
- If documentation edits are large or mostly wording/verbosity changes, a human must decide whether clarity gains justify translation and review cost.
- If a patch is primarily a workaround, a human must decide whether temporary mitigation is acceptable versus requiring a root-cause fix.
- If feature-like changes are proposed near feature freeze, a human must decide whether to defer to a later release.
- If default shortcuts or launch-target behavior changes, a human must validate OS conflicts and expected user experience.
- If a PR mixes main fixes with unrelated changes, a human must decide whether to split for safer review and regression control.
