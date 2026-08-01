# NEA Project Agent Rules

## Mission

Build an evidence-first, self-hostable DAO3 / dao3.fun compatibility path that can run a real map with both client and server Script Runtimes.

## Repository boundaries

- `Frontend/demo-map/`: executable demo, importer, runtime integration, and validation.
- `Middleware/runtime-compat/`: ABI catalogs, reports, fixtures, and conformance tests.
- `Backend/local-player/`: Player hosting and compatibility backend.
- `Evidence/preservation-dump/`: bounded capture/export tooling.
- `Evidence/origin/`, `Shared/mudb/`, `Evidence/dao3-docs-mirror/`, `Evidence/dump/`: evidence and historical inputs, not new runtime architecture.
- `Evidence/works/`: public catalog and ignored private work sources.
- `Docs/`: project governance, architecture, progress, and AI context.
- `Docs/zh/`: Chinese user-facing guidance only.
- `tools/`: maintenance helpers.

## Safety and evidence

- Never publish or modify private captures, credentials, browser state, token-bearing URLs, private maps, ignored reference worktrees, `Evidence/dump/private/`, `Evidence/works/private/`, `.workspace/`, or `NEA-Project.7z`.
- Do not invent historical behavior. Use `compatible`, `partial`, `recovered-only`, `declared-only`, or evidence-deferred classifications.
- Keep evidence, generated reports, and executable implementation separate.
- Do not use names, paths, imports, or runtime dependencies belonging to external bypass/外挂 projects in scripts or implementation code.
- Replace external references with a neutral in-repository evidence path, an explicit configuration input, or a rebuilt local fixture before implementation work continues.

## Change discipline

- Read the relevant package docs and tests before editing.
- Work on one task ID at a time and keep the change within its declared scope.
- Prefer the smallest root-cause fix; avoid unrelated refactors and broad cleanup during feature work.
- Add or update focused conformance tests for runtime behavior changes.
- Do not run broad tests or builds unless the user asks; report the exact commands that should be run.
- Do not commit, push, reset, clean, rebase, change remotes, or move repository directories without explicit user approval.
- Follow the code standards below for all new or substantially modified code; with no lint/format tooling installed, manual review enforces them.

## Patch tool requirement

- After implementation approval, all file edits must use `D:\Projects\Gaming\NEA-Project\tools\apply_patch.ps1`.
- Do not edit files with `Set-Content`, `Out-File`, `Add-Content`, direct redirection, ad hoc Python/Node rewrites, or shell replacement commands.
- The patch wrapper preserves UTF-8 input through a temporary UTF-8 file before invoking Python; do not bypass it on Windows.
- Before applying a patch, confirm approval, allowed write scope, and exclusion of private/generated/forbidden paths.
- After applying a patch, inspect `git diff --check`, `git diff --stat`, and only the changed regions required for verification.
- Analysis-only tasks must not call the patch tool.

## Analysis and refactor gate

- Architecture-analysis tasks are read-only. Do not modify source code, tests, generated outputs, package manifests, configuration, or directory structure during analysis.
- Before editing, produce a responsibility map, dependency map, mutable-state map, external-IO map, test-gap list, and one smallest safe implementation task.
- Do not approve a refactor merely because a file is large. Identify cohesive responsibilities, hidden initialization order, event listeners, timers, process lifecycle, and circular-dependency risks first.
- Do not split a large file into thin forwarding modules. Every extracted module must own a cohesive responsibility and reduce coupling.
- Do not rename or delete a referenced path until callers, generators, tests, reports, documentation, ignore rules, and provenance have been audited.
- After analysis, stop and request approval before implementation when the task is an architecture or migration phase.

## Code standards

The repository has no ESLint/Prettier configuration; this section is the format and behavior authority for hand-written code. Vendored evidence — `Shared/mudb/`, `Backend/local-player/archive/`, `Backend/local-player/runtime/`, generated catalogs, manifests, and reports — is exempt and must not be reformatted.

### Formatting

- Follow `.editorconfig`: UTF-8, LF line endings, final newline, no trailing whitespace; 2-space indentation (4 for `.ps1`).
- Keep lines at or below 80 characters where practical; do not extend existing long lines as part of unrelated edits.
- Match the conventions of the file being edited (indentation, quotes, semicolons).

### JavaScript / ESM

- New hand-written code is ESM only (`import`/`export`); no CommonJS.
- Import standard library modules with `node:` prefixes, grouped before local imports.
- Prefer named exports; avoid default exports in new modules.
- Prefer `const`; use `let` only when rebinding; never `var`.
- Use `async`/`await` instead of promise chains and callbacks.
- Follow the repo style: double quotes, no semicolons (as in `Backend/local-player/src/`).
- Top-level module code must be limited to `const` initialization; no other side effects at import time.

### Naming

- Files and directories: kebab-case (e.g. `client-runtime.mjs`).
- Functions and variables: camelCase.
- Classes, types, and interfaces: PascalCase.
- Module-level constants: UPPER_SNAKE_CASE.
- Booleans: `is`/`has`/`can` prefixes.
- Names state intent; no single-letter names outside tight loops and math.

### TypeScript

- New or substantially modified `.ts` code must compile under `strict`.
- Public APIs need explicit types; add JSDoc when the contract is non-obvious.

### Structure

- Keep interface, application/service, evidence/data access, and utility concerns separated. Do not place protocol parsing, business decisions, filesystem access, and HTTP/Player orchestration in one function.
- Keep functions focused on a single responsibility; prefer small cohesive modules and a clear seam over speculative abstraction.

### Errors and validation

- Prefer guard clauses and early returns; keep conditional nesting at three levels or less.
- Never swallow exceptions. Distinguish expected domain failures from system failures, preserve the cause, and emit useful structured diagnostics at important boundaries.
- Validate all external input for presence, type, format, and range.
- Add boundary validation, timeout behavior, and failure handling for filesystem, network, subprocess, and runtime bridge calls.
- Fail loudly on programmer error; degrade gracefully only for expected domain failures.

### Dependencies

- New dependencies require a short justification, a version/compatibility check, and confirmation that the standard library or an existing dependency is insufficient.

### Comments and diagnostics

- Write comments in English; explain why, not what.
- Explain non-obvious compatibility workarounds at the call site.
- Never leave commented-out code or debug output.
- Logs must be actionable without exposing private data or tokens.

### Tests

- Use the Node built-in test runner (`node:test`) for new tests.
- Place focused tests next to the code they cover (e.g. `src/<pkg>/test/`).
- Add a regression test for every fixed failure; do not mark a task complete based on a happy path alone.

## Maintainability limits

- Keep ordinary business functions at or below 80 lines and utility functions at or below 50 lines. Split longer functions unless the file is generated or a compatibility adapter requires a documented exception.
- Keep ordinary implementation files at or below 500 lines. Generated catalogs, manifests, minified bundles, and archived evidence are exempt but must not be edited as hand-written source.
- Extract shared logic when similar behavior appears three or more times. Do not copy and paste protocol, validation, logging, or error handling branches.
- Replace magic numbers and strings with named constants or configuration.
- Do not use these limits to justify speculative over-abstraction. Prefer small cohesive modules and a clear seam over a framework.
- Enforcement: `pwsh tools/check-maintainability.ps1` lists tracked files over 500 lines; run it from the repo root.

## Evidence migration gate

- A neutral evidence fixture may contain only facts traceable to an approved local source, capture, declaration, or reviewed artifact.
- Never replace missing evidence with plausible defaults, inferred values, copied external source, or path-existence claims.
- Missing evidence must remain `evidence-blocked`, `evidence-deferred`, `partial`, or another honest classification.
- Never hand-edit generated ABI, compatibility, or evidence reports to remove old provenance or make counts look complete. Change the generator or evidence input, then regenerate.
- Every evidence fixture must identify its source class, redaction status, public/private status, and reproducibility limits.

## AI self-review before completion

Before reporting completion, check:

1. Is each changed function within the size limit, or is the exception documented?
2. Did any similar logic get copied instead of extracted?
3. Are external inputs, IO, timeouts, and errors handled explicitly?
4. Are magic values named and configuration-driven where appropriate?
5. Is nesting shallow and are branches easy to test?
6. Are logs actionable without exposing private data or tokens?
7. Is there a focused test or a clearly recorded validation blocker?
8. Did the change preserve the repository layer boundaries and evidence policy?
9. Does the code follow the Code standards section (formatting, naming, ESM, structure, comments)?

## Current priority

Make the real-map client/server runtime loop work before broad physics coverage, full API completion, or frontend polish.

## Language requirement

- All AI-assisted development instructions, plans, code reasoning, task descriptions, commit-style summaries, and validation reports must be written in English.
- Code identifiers, tests, diagnostics, and technical documentation created for the implementation should use English unless a user-facing product requirement explicitly requires another language.
- Chinese project guidance belongs under `Docs/zh/`; do not mix Chinese prose into the English engineering docs.
- If the user asks a question in Chinese, the agent may answer the user in Chinese, but any development artifact or instruction intended for another AI agent must remain in English.

## Task completion format

End implementation work with:

```text
Changed:
- path: short description

Validation:
- command: result or not run

Risks:
- unresolved behavior or privacy concern

Next:
- one concrete follow-up task
```
