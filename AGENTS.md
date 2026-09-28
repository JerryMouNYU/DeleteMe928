# AGENTS.md

## Project overview

This is a Unity project named `DeleteMe928`, using Unity `6000.6.0f1` (Unity 6.0.6f1), the Universal Render Pipeline, and the Input System package. The current project is a first-person platforming prototype: the reusable player prefab handles movement, camera look, jumping, and interaction; `Lava` returns the player to its recorded starting transform.

The main project areas are:

- `Assets/Scenes/` - `SampleScene.unity` and `PlatformerLevel.unity`.
- `Assets/Player/` - `FirstPersonController.prefab` and its scripts.
- `Assets/LevelObjects/` - level scripts and materials, including `Lava.cs`.
- `Assets/EverythingElse/FirstPersonInputActions.inputactions` - the input action asset.
- `Assets/Settings/` - URP and volume assets.
- `ProjectSettings/EditorBuildSettings.asset` - the enabled build-scene list and input-actions reference.

Both `SampleScene` and `PlatformerLevel` are enabled in Build Settings, in that order. `PlatformerLevel` contains the current platform layout, start pad, lava, goal, platforms, lighting, and global volume visible from the scene asset. Read the relevant scene and prefab before changing serialized references or gameplay flow.

## Before changing code or assets

Trace the relevant gameplay flow first. Start with the scene or prefab that owns the behavior, then read the attached MonoBehaviours and the input action used by the behavior. Do not infer a system from a class name alone; confirm its scene, prefab, serialized fields, layer masks, trigger setup, and references.

Preserve the project's existing conventions. Observed code uses one public PascalCase class per script with matching filenames, private fields (including serialized fields) in camelCase, Unity lifecycle methods, and focused scripts under feature folders such as `Assets/Player/Scripts` and `Assets/LevelObjects/Scripts`. Existing assets use descriptive PascalCase names for prefabs, scenes, materials, and level objects. Match nearby code and asset naming rather than introducing a new scheme.

The player uses `FirstPersonInputActions` and its `Player` action map. The asset defines `Move`, `LookMouse`, `LookStick`, `Interact`, `Interact2`, `Crouch`, `Jump`, `Previous`, `Next`, and `Sprint`; keyboard movement includes WASD and arrow keys, with gamepad and other bindings also present. `Controls.cs` is the current access point for movement, look, jump, and interact values. Extend the existing action asset and access pattern when an input change is needed; do not add a parallel input system without agreement.

Keep game rules and state separate from presentation, input plumbing, and effects where practical. Prefer a small, focused change in the existing owner of a behavior. Keep changes playable and incremental; avoid unrelated refactors, broad renames, or speculative features. Preserve explicit design decisions. If a different approach would materially improve maintainability, performance, testability, or editor workflow, explain the benefit and tradeoff briefly and get agreement before changing the requested design.

Ask grouped questions when unanswered choices would materially affect the design or implementation, and recommend sensible defaults. For minor uncertainties, state the assumption in the task summary and continue. Before editing scenes, prefabs, materials, or serialized fields, identify any required Inspector, hierarchy, trigger, layer, prefab, or asset setup and call it out explicitly.

## Verification

Use the tools available for the change and report exactly what was verified. Code inspection, compilation, static searches, and other checks are not a substitute for running the game. Distinguish:

- Code or asset checks: inspect diffs, references, serialized data, and any available compiler/test output.
- In-engine playtesting: open the project in Unity `6000.6.0f1`, enter Play Mode, and exercise the affected scene and input path. Only claim this was done if it was actually done.

The repository contains the Unity Test Framework package (`com.unity.test-framework`), but no project-authored test files were found during inspection. No repository-defined CLI build or test script was found. Use the Unity Editor's Test Runner and Build Settings/Build Profiles only when appropriate, and record the exact result rather than inventing a command. The verified build-scene configuration is `SampleScene` followed by `PlatformerLevel`.

## Task handoff

End each task with:

1. A concise summary of the behavior or asset change.
2. The files changed.
3. The checks performed and, separately, how to test the result in Unity Play Mode.
4. Any required manual scene, prefab, asset, Inspector, layer, or editor setup.
5. Known limitations, unverified areas, or follow-up work.
