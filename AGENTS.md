# SimplifAI Repository Instructions

## Project Identity
- Build **SimplifAI** as a **Tauri v2 desktop application**.
- The product is **desktop-first**, not browser-first.
- Use a **React + TypeScript** frontend inside the Tauri shell.
- Use **Rust** in `src-tauri` for desktop-native behavior.

## Core Stack
- Tauri v2
- React
- TypeScript
- Vite
- Tailwind CSS
- Framer Motion
- Lucide React
- Mermaid.js or equivalent for diagrams/flow visuals

## Architecture Rules
- Frontend lives in standard React app structure.
- Native desktop logic lives in `src-tauri`.
- Keep code modular and split by responsibility.
- Prefer folders such as:
  - `src/components`
  - `src/views`
  - `src/services`
  - `src/context`
  - `src/types`
  - `src-tauri`

## Product Rules
- This app must not contain dead UI.
- Every important button, toggle, slider, menu, and control must do something real:
  - state change,
  - actual handler,
  - real service call,
  - or a clearly explained fallback behavior.
- No fake critical-path behavior.
- No broken routes.
- No placeholder-only core features.

## Desktop-First Rules
- Prefer native desktop patterns where appropriate.
- Support drag-and-drop into the app window.
- Support native file picker dialogs.
- Design for local file handling and local project storage.
- Do not treat this as a plain web app wrapped at the last minute.

## Build Behavior
- Inspect the repository before making major structural decisions.
- Reuse existing patterns before inventing new ones.
- Keep changes coherent and maintainable.
- Do not rewrite unrelated parts of the codebase without cause.
- Preserve user changes.
- Never use destructive git commands unless explicitly requested.

## Quality Standard
- Finish working flows, not just scaffolding.
- Verify the app builds before calling work complete.
- Run relevant checks when available:
  - build
  - typecheck
  - lint
  - tests
- Fix obvious breakages before stopping.

## UI / UX Rules
- Avoid generic, lifeless UI.
- Keep layouts intentional and readable.
- Prioritize clarity, speed, and usability.
- Preserve a coherent visual direction across the app.
- If a design system already exists, follow it.

## Source of Truth
- Treat `docs/simplifai-spec.md` as the product source of truth.
- Treat `plans/build-plan.md` as the execution plan.
- If implementation details are missing, make reasonable assumptions consistent with:
  - Tauri v2 desktop-first architecture
  - local-first behavior
  - no-fluff UX
  - safe, grounded AI behavior
  - the dual-persona SimplifAI concept

## Deliverable Standard
Before considering major work complete, ensure:
- the app runs
- key flows are wired
- core views are accessible
- major errors are fixed
- README setup steps remain accurate