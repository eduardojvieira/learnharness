Objective: Spanish (neutral, "tú") LearnHarness UI + Spanish-teaching agent, and an unsigned macOS arm64 DMG.
Branch: `es` on the user's fork. Delivery: single branch, DMG via `release.yml` workflow_dispatch (macos-26). Runner: `npm test` (node:test via tsx), `npm run typecheck`, `npm run lint`.

## Specs

- S1: "Puedes hacer una traduccion en español de lo siguiente" — translate every user-visible UI string (React components, app routes, Electron menus/dialogs, user-facing errors) to neutral Spanish. Model-facing text (tool schemas/descriptions, tool results), identifiers, storage keys, file formats and log lines stay unchanged.
- S2: The agent teaches in Spanish (assumption: one language rule in the base system prompt; the doctrine fragments stay in English).
- S3: "generar los archivos de release (necesito el dmg para macos arm64)" — produce the arm64 DMG from the fork via the manual (unsigned, unpublished) `Desktop release` run on macos-26.
- S4: `npm run typecheck`, `npm test` and `npm run lint` pass after the translation (tests asserting English UI copy are updated to the Spanish copy).

## Tasks

- T1 (S1) Graph area — delegated (parallel writer A) — done, typecheck clean
- T2 (S1) Settings, keys, keyboard, Electron — delegated (parallel writer B) — done; parent routed thinking levels through THINKING_LABEL
- T3 (S1) Chat shell, sessions, home, app/ — delegated (parallel writer C) — done
- T4 (S1) Plan, practice, questions, artifacts, ai-elements — delegated (parallel writer D) — done
- T5 (S2) Agent language rule — inline — done (src/agent/system-prompt.md `## Language`; prompt tests 39/39 pass)
- T6 (S4) Test/typecheck/lint seam — delegated (context backstop: ~275 copy assertions in 50 test files) — done: TZ=UTC npm test 2339/2339, typecheck 0 errors, lint clean
- T7 (S3) Fork, push, manual release run, download DMG — inline

## Log

- L1 (2026-10-27): "Puedes hacer una traduccion en español de lo siguiente y generar los archivos de release (necesito el dmg para macos arm64): https://github.com/amosblomqvist/learnharness"
- L2 (2026-10-27): User chose "Fork + GitHub Actions (Recomendado)": authorized using their `gh` session (account eduardojvieira) to fork, push the translated branch and manually run "Desktop release" on macos-26; DMG is unsigned, no release published.
- L3: The DMG cannot be built on Linux (actool/Xcode 26 for the icon; docs forbid cross-building the Next standalone server). Writers split by disjoint file surfaces (Writer trigger a: parallelism).
- L4: After T1–T5: 84 files changed; typecheck 1 error (tests/tooltip-mechanism.test.tsx pins old AUTO_UPDATE_HINT); lint clean; npm test 277/2339 fail (2 baseline). Kept English on purpose: BYOK refusals in src/core/settings.ts (persisted, prefix-matched), electron ChatGPT/subscription publicError texts classified by English regex in src/agent/*-stream.ts, formatChainLine, session-markdown, "Untitled session" store fallback.
- L5: Known flaky (baseline, not caused by the translation): tests/chatgpt-auth.test.ts:614 under full-suite load; tests/render-sidebar.test.tsx:84 is TZ-dependent and passes under TZ=UTC.
