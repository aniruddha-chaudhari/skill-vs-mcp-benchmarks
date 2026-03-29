# OpenCode Agent-Browser Eval Framework

This repository bundles the Bun-powered backend that drives agent-browser evaluations. The workspace is organized so evaluation assets and OpenCode docx helpers are easy to locate.

## Workspace Layout

- `src/` - Bun server entrypoint (`src/index.ts`), shared helpers, and types that power backend eval services.
- `eval/` - evaluation tooling and outputs. Includes `eval/run-eval.ts`, `eval/run-task-demo.ts`, `eval/eval-config.json`, the framework under `eval/src/eval/**`, `eval/eval-results/`, and `eval/eval-results.json`.
- `.opencode/skills/docx/refrences/` - docx helper scripts (`create-docx.js`, `create_docx.js`, `edit-docx.py`) colocated with the docx skill references.

## Evaluation Workflow

1. Install workspace dependencies at the root:

```bash
bun install
```

2. Start Chrome with remote debugging enabled (for example by running `node src/launch-chrome-standalone.cjs` or launching Chrome manually with `--remote-debugging-port=9222`).

3. Confirm port `9222` is open:

```powershell
# Windows
netstat -ano | findstr :9222
```

```bash
# Linux/macOS
netstat -tlnp | grep :9222
```

4. List and run eval tasks:

```bash
bun run eval/run-eval.ts --list
bun run eval/run-eval.ts EVAL_ID
```

Outputs are written under `eval/eval-results/<timestamp>/`, and the latest summary is written to `eval/eval-results.json`.

5. `eval/run-task-demo.ts` now prints a pointer to the main CLI because the old MockAgent helpers were retired.

## OpenCode docx Helpers

Any loose docx scripts should live under `.opencode/skills/docx/refrences/`. If needed, update `.opencode/skills/docx/SKILL.md` to point to the new helper locations.

## Running the Backend

- `bun run src/index.ts`
- `bun run --hot src/index.ts`

`tsconfig.json` should include both `src/**/*` and `eval/src/**/*` so tooling can resolve moved eval helpers.
