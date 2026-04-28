---
name: create-sp-feature
description: Create and implement a new Unity feature from a user-provided feature description in its own folder using Clean Code and MVP Passive View. Use when the user asks for create-sp-feature/create_sp_feature/new feature scaffolding or implementation with strict structure (`Services`, `Providers`, `Presenters`, `View`, `Factories`, `SO`), interface-first design, `IServicePreloader` warmup initialization, and optional per-frame logic via R3 `Observable.EveryUpdate`.
---

# Create SP Feature

Implement a feature from description, keep it isolated, and leave integration for later.

## Workflow

1. Request a concrete feature description if it is missing.
2. Choose a feature root folder in the current domain and create a dedicated feature directory.
3. Scaffold strict internal folders and interface-first contracts.
4. Implement MVP Passive View classes with clear responsibilities.
5. Add preloader entrypoint via `IServicePreloader` and `Warmup()`.
6. Leave the feature disconnected from composition root, scene setup, and global registries.

## Required Input

Require a short feature brief before coding:
- business goal
- user actions
- expected states/results
- tick needs (if any)
- data to persist (if any)

If this is not provided, ask for it in one concise question.

## Dependency Rule

This skill depends on reactive and async packages:
- reactive: prefer `R3`; fallback to `UniRx` when the project already uses it
- async: `UniTask`

Before implementation, verify these packages/usages exist in the project.
If any required package is missing:
1. first propose installing the missing package(s) to the user
2. only implement a non-package alternative if the user explicitly refuses installation

Do not silently switch to alternatives while package installation is still acceptable.

## Folder Contract

Create the feature in its own folder and keep this structure:

```text
<FeatureRoot>/
  Services/
  Providers/
  Presenters/
  View/
  Factories/
  SO/
```

Use `SO/` for ScriptableObject configs only.

## Architecture Rules

Apply these rules to every implementation:

1. Use MVP Passive View.
2. Keep View passive: render state and emit UI events only.
3. Keep Presenter as orchestration layer: subscribe, map state, and call view/service.
4. Keep Service focused on business logic and state transitions.
5. Define interfaces for service, view, and presenter.
6. Add provider/factory interfaces when the abstraction is non-trivial or reused.
7. Use constructor injection; avoid hidden global lookups inside the feature.
8. Keep classes small and name by responsibility.

## Tick Rule (R3)

When per-frame behavior is required, use R3 semantics:

- use `Observable.EveryUpdate()`
- keep subscription lifecycle in disposables
- dispose subscriptions on teardown
- avoid `Update()` in presenter/service logic

If `R3` is not available but `UniRx` is installed, use the equivalent UniRx every-frame observable.
If neither is available, follow the Dependency Rule and ask to install before using alternatives.

## Initialization Rule

Create a feature initializer/preloader class that implements:

```csharp
IServicePreloader
UniTask Warmup()
```

Use the project signature from `Assets/Scripts/Meta/Interfaces/IServicePreloader.cs` (`Warmup`, not `WarmUp`).
Use this instead of `IStartable`.
If `UniTask` is unavailable, follow the Dependency Rule and propose installation first.

## Non-Integration Rule

Do not connect the new feature immediately:

- do not register it in global installers/composition roots
- do not wire it into scenes/prefabs unless explicitly requested
- do not modify unrelated bootstrap code

Return a short "how to integrate later" note only.

## When Not To Use

Do not use this skill in these cases:

1. The project uses ECS/DOTS as the primary architecture for this domain. In that case, this OOP MVP structure is not the right fit.
2. The task is only a tiny hotfix in an existing feature and does not need new folder scaffolding.
3. The team already has a strict alternative feature template that conflicts with this folder contract.
4. The request requires immediate runtime wiring in composition roots/scenes as part of the same task.
5. The feature is engine-level infrastructure (rendering, low-level networking, platform bootstrap) where MVP UI boundaries are irrelevant.

## Output Checklist

Before finishing, verify:

1. Feature has its own root folder.
2. Required subfolders exist.
3. Service/view/presenter interfaces exist.
4. MVP Passive View boundaries are respected.
5. Tick logic uses `Observable.EveryUpdate()` if needed.
6. Preloader implements `IServicePreloader` + `Warmup()`.
7. No runtime integration changes were made.
