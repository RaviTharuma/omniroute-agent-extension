# AGENTS.md

Instructions for AI agents working in this repository.

## Start Here

Before editing, read:

1. `AI.md` — fast project handoff and key function map.
2. `ARCHITECTURE.md` — extension data flow and prompt-tool design.
3. `README.md` — user-facing behavior and commands.
4. `CONTRIBUTING.md` — local checks and contribution rules.

## Must-Do Documentation Rule

If you change source behavior, update docs in the same change.

Use this mapping:

| Change type | Docs to update |
|---|---|
| User-visible command/setup/model behavior | `README.md` |
| Provider flow, tool routing, prompt-tool logic | `ARCHITECTURE.md` |
| File layout, key function names, scan paths, pitfalls | `AI.md` |
| Dev workflow, tests, contribution process | `CONTRIBUTING.md` |
| Package scripts/deps | `README.md` Development section and `CONTRIBUTING.md` if relevant |

Do not leave code/docs inconsistent.

## Core UX Constraint

Keep model switching normal:

```text
/model <model-id>
```

Do not introduce duplicate providers or separate manual prompt-tool model lists unless user explicitly asks.

The provider should remain:

```text
omni
```

## Prompt Tool Constraint

Chat-only models should use prompt-emulated tools automatically when:

- model id/name/provider or OmniRoute `owned_by` contains `-web`
- or raw `models.json` model entry has `tool_calling:false`

Do not rely only on Pi runtime `Model` for custom metadata. Pi strips unknown fields; use raw `models.json` when needed.

## Testing (HARD)
Sources: https://x.com/anshnanda/status/2101627891721371971 · https://x.com/nimsbh_ai/status/2102083469362790401 · https://x.com/imrobertjames/status/2100787901701456057

- NEVER write unit tests after you write code.
- Highly prefer E2E tests as the sole testing mechanism. Use them to verify complex features work. At the end of E2E tests, produce a verifiable and repeatable artifact.
- If you must test a system in isolation, FIRST write down all the ways it could fail, THEN write the code.
- When writing E2E tests, do not pick the simplest possible scenario to prove it works — pick a medium-to-hard scenario (models love to cheat).
- Tautological tests considered harmful.
- Change-detector tests considered harmful.
- Do not create regression tests for bug fixes without a genuine gap in behavior testing.

Keep genuine integration, telemetry, and routing-correctness tests. Delete low-signal unit tests that E2E would already cover.

## Test Before Reporting Done

Prefer proving changes with E2E (linked Pi/OMP host + `/omni` flows) ending in a verifiable artifact. Keep existing routing/integration Node tests when they assert real failure modes.

Also run:

```bash
npm run typecheck
npm run smoke
npm test
```

If tests cannot run, report exact command and failure.

## Edit Guidance

- Prefer small targeted edits.
- Keep comments on non-obvious functions.
- Preserve `/omni setup`, `/omni sync`, `/omni dashboard` behavior unless user asks to change it.
- Keep prompt-tool format and parser docs in sync.
- If adding files, update `AI.md` file map.

## Important Files

| File | Why important |
|---|---|
| `index.ts` | Extension implementation. |
| `AI.md` | AI scan guide; update when project structure/function map changes. |
| `ARCHITECTURE.md` | Data flow and tool routing docs. |
| `README.md` | User-facing documentation. |
| `CONTRIBUTING.md` | Dev/test workflow. |
| `package.json` | Pi extension metadata and scripts. |
