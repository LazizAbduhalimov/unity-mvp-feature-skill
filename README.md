# Unity Agent Skills

This repository contains practical skill definitions for an AI coding agent working on Unity projects.

The goal is to give the agent a clear, repeatable way to scaffold new features and apply a consistent C# coding style. The skills are intentionally focused, lightweight, and easy to reuse across different game modules.

## Repository Purpose

Use these skills as instruction packs for agent-driven development tasks such as:

- creating a new feature in a clean, isolated structure
- following MVP Passive View boundaries
- keeping initialization predictable with preloader-based warmup
- applying a concise, low-ceremony C# code style during refactors

## Included Skills

### `create-sp-feature`

A feature creation skill for Unity that guides the agent to implement new functionality in its own folder with strict internal structure:

- `Services`
- `Providers`
- `Presenters`
- `View`
- `Factories`
- `SO`

It emphasizes interface-first design, clear responsibility boundaries, and `IServicePreloader` + `Warmup()` initialization. It also includes rules for update-loop logic via reactive patterns (`Observable.EveryUpdate`) when needed.

### `no-null-check-code-style`

A concise C# style skill aimed at reducing unnecessary ceremony in Unity code.

It encourages direct constructor assignment, fewer repetitive argument guards, shorter branching patterns, and readability-first refactors while preserving behavior. The style is especially useful when cleaning up verbose service/presenter classes generated over multiple iterations.

## Typical Agent Workflow

1. Read the feature request.
2. Apply `create-sp-feature` to scaffold and implement the feature in isolation.
3. Apply `no-null-check-code-style` to simplify and normalize the resulting C# code.
4. Leave integration into global composition/scene wiring for a separate explicit step.

## Notes

- These skills are designed for Unity-focused code generation and refactoring.
- They are complementary: one defines architecture flow, the other enforces concise implementation style.
- You can extend this repository with additional skills for testing, CI checks, or project-specific conventions.
