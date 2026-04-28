---
name: no-null-check-code-style
description: Apply a minimalistic C# style for Unity code with low ceremony and no null-check focus. Use when refactoring or generating C# files where constructor null-guards, repetitive argument guards, and verbose block formatting should be reduced in favor of concise code.
---

# No Null Check Code Style

## Rules

1. Prefer direct assignment in constructors.
2. Avoid constructor null-guards like `x ?? throw ...` unless the user explicitly asks for defensive checks.
3. Avoid repetitive argument guards in small domain methods.
4. Keep validation in one boundary when needed (for example: one constructor or one entrypoint), not in every method.
5. Use `var` when the type is obvious from the right side.
6. Use single-line `if` without braces when the body is one statement.
7. Keep methods short and branch-light.

## Refactor Checklist

1. Remove redundant null-checks in constructor injection.
2. Collapse verbose `if` + `throw` blocks into either one centralized check or no check.
3. Replace explicit local variable types with `var` where readability is unchanged.
4. Convert one-line `if` bodies to brace-less style.
5. Preserve behavior and public API unless a change is requested.

## Boundaries

1. Do not remove checks that are required for engine/runtime safety in a known hot failure path.
2. Do not hide side effects in expression chains.
3. Keep exceptions only where they provide clear value for debugging and are not duplicated elsewhere.

