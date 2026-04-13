# SimplifAI Repository Instructions

## Project Identity
- Build **SimplifAI** as a **local-first Tauri v2 desktop application**.
- Use a **React + TypeScript** frontend.
- Use **Rust** in `src-tauri` for desktop-native behavior.
- This project is **desktop-first**, not browser-first and not cloud-first.

## Product Source of Truth
- Treat `docs/simplifai-spec.md` as the product source of truth.
- Treat `plans/build-plan.md` as the execution plan.
- If implementation details are missing, make reasonable choices that stay consistent with the spec and plan.

## Core Stack
- Tauri v2
- React
- TypeScript
- Vite
- Tailwind CSS
- Framer Motion
- Lucide React
- Mermaid.js or equivalent where useful

## Architecture Rules
- Keep code modular and split by responsibility.
- Prefer this structure unless the repo evolves differently for good reason:
  - `src/components`
  - `src/views`
  - `src/services`
  - `src/context`
  - `src/types`
  - `src-tauri`
- Keep model/provider logic out of UI components.
- Route AI behavior through service abstractions.

## V1 Scope Rules
- Local-first only.
- Local storage / SQLite-style persistence.
- Local exports only.
- No cloud sync unless explicitly added later.
- No telemetry, background uploads, or remote-first behavior.
- Do not introduce orgs, billing, public share links, or collaboration unless explicitly requested later.

## AI Routing Requirements
- Support these model targets through adapters/services:
  - Gemini 3.1 Pro
  - Gemini 3.1 Flash Lite
  - Gemini 3 Flash with `thinking_config=medium`
  - MiniMax M-2.7
- Use task-based routing in services, not hard-coded UI logic.

## Desktop-First Behavior
- Support drag-and-drop into the app window.
- Support native file picker dialogs.
- Support local project storage and local source reuse.
- Use desktop-appropriate save/export behavior.
- Do not treat this as a normal web app wrapped at the end.

## Product Quality Rules
- No dead UI.
- No fake critical-path buttons.
- Every important control must do something real:
  - state change
  - actual handler
  - real service call
  - or a clearly explained fallback
- No broken routes.
- No placeholder-only core flows.

## Safety and Grounding Rules
- Keep outputs source-faithful by default.
- Do not invent safety-critical facts when the source is missing them.
- Surface ambiguity clearly with warnings or unverified states.
- For physical workflow content, prefer literal, concrete, safety-first wording.

## Code Change Rules
- Inspect the repo before making major structural decisions.
- Reuse existing patterns before inventing new ones.
- Keep edits coherent and maintainable.
- Do not rewrite unrelated code without cause.
- Preserve user changes.
- Never use destructive git commands unless explicitly requested.

## Completion Standard
Before calling major work complete:
- the app must run
- key flows must be wired
- major views must be accessible
- obvious breakages must be fixed
- README steps must remain accurate

## Verification
- Run relevant checks when available:
  - build
  - typecheck
  - lint
  - tests
- Fix obvious failures before stopping.

## Response Style
- Be concise.
- Put the direct answer or deliverable first.
- Ask only necessary clarifying questions.
- When providing code updates, prefer complete file outputs over partial snippets if the user is expected to copy/paste.
- For terminal instructions:
  1. provide the exact `cd` command first
  2. provide the command to run second
  3. state whether a new PowerShell window is required
