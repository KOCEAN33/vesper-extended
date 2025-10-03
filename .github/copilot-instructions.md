# Copilot instructions for this repo

This repository is a VS Code color theme extension (no runtime code). The single source of truth is the theme JSONC in `themes/Vesper-extended-color-theme.json`. Use this guide to make precise, minimal edits and validate quickly.

## Architecture and intent

- Extension type: declarative theme (contributes.themes) defined in `package.json` → `themes/Vesper-extended-color-theme.json`.
- Packaging only includes root files and `themes/**`; `demo/**` is intentionally excluded via `.vscodeignore` and exists for visual verification in the editor.
- Palette and accents used across the theme (maintain consistency):
  - Backgrounds: `#101010`, surfaces `#161616`/`#1C1C1C`.
  - Accents: `#FFC799` (primary), `#99FFE4` (added/ok), `#FF8080` (error), neutrals `#A0A0A0`/`#abb2bf`.

## Key files

- `package.json`: extension metadata, engine range, and theme contribution path.
- `themes/Vesper-extended-color-theme.json`: JSON with comments (JSONC) defining `colors` and `tokenColors`.
- `.vscodeignore`: excludes `demo/**` from the packaged VSIX.
- `demo/*`: example files (ts/js, jsx, py, rb, yml, tf, scss/css, erb, etc.) for eyeballing tokenization.

## Local workflow (build, test, iterate)

- Quick iteration: open this repo in VS Code, set theme to “Vesper Extended”, edit `themes/Vesper-extended-color-theme.json`, then run “Developer: Reload Window”. Use “Developer: Inspect Editor Tokens and Scopes” to map text → scopes.
- Extension host: run “Run Extension” (F5) to launch a Development Host and inspect tokens on `demo/*`.
- Package a VSIX: `vsce package` (install via `npm i -g vsce`). Version in `package.json` must be bumped for new VSIX.

## Conventions and patterns

- Keep edits minimal and scoped:
  - UI colors go under `colors` (e.g., `editor.background`, `statusBar.background`).
  - Syntax colors go under `tokenColors` as an array of objects: `{ name, scope, settings: { foreground, fontStyle } }`.
- Scopes are comma‑separated; prefer language‑specific scopes to avoid regressions. Examples already in use:
  - Python: `variable.parameter.function.language.special.self.python`.
  - Rust: `entity.name.lifetime.rust`, `variable.language.rust`.
  - Java: `storage.type.annotation.java`, `keyword.operator.instanceof.java`.
  - JS/TS: `support.type.object.console`, `keyword.operator.optional`, `variable.other.constant`.
  - CSS/SCSS: `support.constant.property-value.scss`, `keyword.operator.scss`.
- Style intent: use `#FFC799` for emphasis/keywords/constants, `#FF8080` for errors/deletions, `#99FFE4` for additions/positives/ops, neutrals for punctuation and structure.

## Adding or adjusting token rules (example)

Add a targeted rule instead of broad wildcards:

- Before: missing highlight for YAML keys.
- Change: add
  - name: `yaml keys`
  - scope: `entity.name.tag.yaml`
  - settings.foreground: `#FFC799`
    Place near related language rules to keep the file organized.

## Gotchas

- File is JSONC (comments allowed). Keep trailing commas out to avoid packaging issues.
- Do not rename `contributes.themes[0].path` or break the label “Vesper Extended”.
- `demo/**` is not published; use it only for local inspection.
- Engines: root `package.json` targets VS Code `^1.8.0` (historic). Update only if you introduce properties requiring newer VS Code.

## Validation checklist

- Visual diff on representative files in `demo/*` after changes.
- Run “Inspect Editor Tokens and Scopes” to confirm the intended scopes are affected and unrelated scopes remain stable.
- Build a VSIX with `vsce package` and ensure the theme loads without errors.

If anything above is unclear or you need more mapped scopes for a language, leave a short note describing the file/snippet and we’ll expand the rules in a follow‑up.
