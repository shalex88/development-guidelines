# General Coding Standard

## Core Principles
- Pedantic approach: warnings and bad practices = bugs
- Fail fast: prefer compilation error over runtime bug
- SOLID principles: Single Responsibility, Open/Closed, Liskov Substitution, Interface Segregation, Dependency Inversion
- KISS & YAGNI: Keep it simple; avoid overengineering
- Rule of three: implement; copy if necessary; if you copy again, refactor (applies to code and architectural patterns)
- Prioritize code readability over cleverness
- Prefer dependency injection for testability and decoupling
- Enforce consistent code formatting and linting rules

## Project Hygiene
- Every project must include a `README.md` with installation, configuration, and run instructions
- Explicit versioning artifact (e.g., `VERSION` file or tagged releases)
- Maintain a concise `CHANGELOG.md`
- Always add scripts for common tasks (build, install, test)

## Simplicity & Safety
- Validate inputs at module/service boundaries
- Fail fast on invariant violations (assertions or guarded checks)
- Prefer explicit error signaling (Result/ Either) for recoverable conditions
- Avoid hidden side effects; make state transitions explicit
- Always initialize variables and members; no uninitialized state

## Error Handling Policy
- Differentiate recoverable vs unrecoverable errors early
- Provide structured error types (code, category, human-readable message)
- Never swallow errors silently; log or propagate

## Logging & Observability
- Use structured logging (key=value) format
- Never log secrets/PII; centralize redaction utilities
- Standard levels: trace, debug, info, warn, error, critical
- Correlate requests/flows with trace/span IDs in distributed contexts

## Performance & Concurrency (General)
- Avoid premature optimization; measure before optimizing
- Prefer non-blocking operations for I/O-bound workloads
- Use timeouts for all external calls (I/O, RPC, DB) to avoid resource starvation
- Be explicit about ownership and lifecycle of shared resources
- Document concurrency assumptions (single-threaded, lock-free, actor model, etc.)

## Security & Data Handling
- Treat all external input as untrusted; sanitize at boundaries
- Enforce least privilege for services/components
- Never log authentication tokens, secrets, or raw personal data
- Use encryption at rest and in transit where applicable

## Configuration
- Prefer declarative config (YAML/JSON/TOML) over hard-coded constants
- Support environment variable overrides for containerized deployments
- Validate config at startup; fail fast on invalid schema

## Testing Strategy
- Test pyramid: unit (fast, isolated) → integration (contracts) → system/end-to-end (behavior)
- TDD cycle encouraged: write failing test → implement → refactor with tests green
- Minimum 80% coverage for new code (focus on meaningful paths, not chasing 100%)
- Tests must be deterministic and hermetic (no external mutable state unless mocked/faked)
- Regressions always accompanied by a new test

## Continuous Integration
- All merges gated by: build success, tests green, static analysis pass
- Run linters and formatters automatically; builds fail on violation
- Include security scanning (dependency vulnerability checks)

## Documentation
- Co-locate high-level architecture docs (`/docs`) and update with major changes
- Public APIs must have usage examples
- Prefer ADRs (Architecture Decision Records) for significant technical decisions

## Dependency Management
- Pin dependency versions where possible to ensure reproducibility
- Remove unused dependencies proactively
- Evaluate license and security impact before adding a new dependency

## Delivery & Release Practices
- Automate build & packaging (single command or pipeline)
- Use semantic versioning (MAJOR.MINOR.PATCH) or a documented alternative

## Code Review Expectations
- Reviews focus on correctness, clarity, and risk—not personal style
- Require at least one reviewer familiar with affected domain
- Small, focused PRs preferred

## Adoption & Exceptions
- Deviations from standards require a documented rationale and time-bounded follow-up to align
- Experimental features may be flagged and isolated until stabilized