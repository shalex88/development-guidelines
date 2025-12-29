# C++ Standard

## Language & Toolchain
- Target a modern C++ standard (currently C++20); review periodically for upgrade.
- Use a standard-conforming compiler; enable high warning levels and treat warnings as errors.
- Apply static analysis in continuous integration.
- Optionally enable runtime sanitizers in debug/test builds for early defect detection.

## Style & Structure
- Stroustrup brace style; consistent indentation
- Naming: PascalCase (types/classes), camelCase (functions), snake_case (variables), trailing underscore for private data members
- One public class/interface per header where practical
- Headers always start with `#pragma once`
- Prefer forward declarations over unnecessary includes (minimize coupling)
- Always add comment on namespace closures

## Compilation Units & Linkage
- No `static` functions in classes, prefer anonymous namespaces for internal linkage
- Keep headers free of implementation details; minimize transitive includes
- Avoid macros except for include guards handled by `#pragma once` and controlled feature flags

## Memory & Resource Management
- Favor the Rule of Zero; if you implement one special member, reassess the others (Rule of Five).
- Avoid raw pointers entirely (owning or non-owning). Use smart pointers.
- Prefer value semantics; use `std::move` only when transferring ownership or avoiding a costly copy.
- Rely on RAII for all resource management (locks, file descriptors, sockets, timers, etc.).

## Error Handling
- Prioritize a result/expected style (`Result<T,E>` or equivalent) for recoverable conditions.
- Use exceptions for unrecoverable failures or violated invariants; destructors should not throw.
- Mark functions `noexcept` when you can confidently guarantee no exception paths.
- Avoid using exceptions for ordinary control flow or value absence.

## Const Correctness & Immutability
- Mark methods `const` when they do not mutate externally observable state.
- Use `constexpr` for compile-time values and simple computations.
- Prefer lightweight non-owning views (`string_view`, `span`) to avoid unnecessary copying.

## Templates & Generic Programming
- Constrain templates with concepts where clarity improves intent.
- Keep generic code proportional to demonstrated needs; avoid speculative over-generalization.
- Prefer straightforward composition; reserve advanced meta-programming for measured performance needs.

## Concurrency
- Start with higher-level abstractions (e.g. managed threads, async primitives) before manual thread management.
- Use standard synchronization primitives (`mutex`, `lock_guard`, `unique_lock`) rather than ad-hoc mechanisms.
- Document shared data ownership and lifetime when concurrency is involved.
- Employ atomics only when lock-free semantics are necessary and understood.

## Performance Practices
- Measure first; optimize based on evidence.
- Prefer standard library algorithms (including ranges) when they improve clarity and maintain performance.
- Reduce avoidable heap allocations; choose appropriate data structures for typical workloads.

## Logging
- Use a consistent logging facility; prefer structured key/value style for machine parsing.
- Minimize logging in hot paths unless it aids active debugging.

## Build & Dependencies
- Maintain a single authoritative build configuration (e.g. modern CMake usage).
- Manage third-party dependencies explicitly and pin versions for reproducibility.
- Keep the environment stateless: avoid relying on hidden global toolchain state.
- Produce appropriately optimized builds for deployment; include symbols or source maps where operationally useful.

## Testing
- Use a consistent testing framework for unit, integration, and system verification.
- Favor descriptive test names that capture scenario and expectation.
- Cover core behavior, key edge cases, and resource ownership semantics.
- Keep tests isolated, deterministic, and fast; external dependencies should be faked or mocked.

## General Practices
- Avoid unnecessary global state; prefer explicit dependencies.
- Keep public interfaces minimal and intention-revealing.
- Limit file-level complexity; refactor when a compilation unit grows unwieldy.
- Prefer clarity over micro-optimizations unless profiling justifies change.

## Passing & Ownership (Simple Guidelines)
- Value (pass/return `T`): small trivial types, or when you need a local copy anyway.
- Const reference (`const T&`): large/expensive to copy and read-only.
- Unique ownership (`std::unique_ptr<T>` by value): callee takes responsibility; caller passes with `std::move`.
- Shared ownership (`std::shared_ptr<T>`): only when multiple independent owners must extend lifetime; pass by value when storing, by const ref for temporary access.
- Observing a shared object: use `std::weak_ptr<T>` (lock when needed) to avoid cycles.
- Non-owning textual input: use `std::string_view` (do not store beyond source lifetime).
- Non-owning contiguous sequence: use `std::span<T>` / `std::span<const T>`; do not store spans long-term.
- Never raw pointers: use references for guaranteed presence; for optional reference use `std::optional<std::reference_wrapper<T>>` or an index/handle.
- Prefer value semantics when designing types; make them cheap to move.
- Choose const ref over value if copy cost is more than trivial and you only read.
- Avoid passing smart pointers when ownership is not changing—prefer views or (const) references.