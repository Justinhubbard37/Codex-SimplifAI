# SimplifAI Build Plan

## Goal

Build SimplifAI as a local-first Tauri v2 desktop application with:

- React + TypeScript frontend
- Rust-backed desktop shell
- SQLite-style local persistence
- local exports only
- AI routing for:
  - Gemini 3.1 Pro
  - Gemini 3.1 Flash Lite
  - Gemini 3 Flash with `thinking_config=medium`
  - MiniMax M-2.7

This plan is written for execution inside Codex.

---

## Phase 1 — Project Foundation

### Objectives
- scaffold the Tauri v2 desktop app
- establish base folder structure
- confirm the app runs locally
- establish the core theme and app shell

### Deliverables
- working Tauri v2 project
- React + TypeScript + Vite frontend
- `src-tauri` Rust shell
- base layout and navigation shell
- theme support:
  - Dark
  - Light
  - Comfort

### Done criteria
- app launches successfully
- frontend renders inside Tauri
- no broken starter routes
- base styling and shell layout exist

---

## Phase 2 — Core Desktop UX Shell

### Objectives
- implement desktop-first interaction patterns
- build the main navigation and primary views
- support local file selection and drag-and-drop

### Deliverables
- Unified Drop Zone
- Intake Flow Panel
- Main Workspace shell
- Project Library shell
- Project Detail shell
- Settings view
- Help / How It Works view

### Desktop behaviors
- drag-and-drop support
- native file picker integration
- desktop-friendly layout behavior
- local project creation flow

### Done criteria
- user can open the app and create a project shell
- user can add source files locally
- primary views are reachable
- no dead navigation

---

## Phase 3 — Data Model and Local Persistence

### Objectives
- define the local data model
- persist projects and outputs locally
- create a clean storage layer

### Recommended persistence direction
- local SQLite database
- local app-managed storage location
- Rust-side or service-side persistence bridge appropriate for Tauri

### Entities
- projects
- source file records
- outputs
- activity logs
- quality flags
- app settings
- provider settings metadata

### Deliverables
- schema or migration setup
- local persistence services
- create/read/update/delete support for projects
- project library data binding
- activity logging hooks

### Done criteria
- projects persist between app launches
- outputs and settings persist
- sample data can be seeded
- deleting a project removes associated local records cleanly

---

## Phase 4 — Input Processing Pipeline

### Objectives
- support required source formats
- normalize source ingestion
- extract content for AI use

### Supported source types
- documents
- images
- audio
- video

### Deliverables
- local file intake pipeline
- source validation
- file cap enforcement
- metadata extraction
- staged processing states
- progress phase UI

### Required UX states
- Extracting source content
- Analyzing content
- Drafting output
- Finalizing output

### Done criteria
- each supported source type can enter the system
- invalid files are rejected clearly
- long-running tasks show phased progress
- source artifacts are attached to projects correctly

---

## Phase 5 — AI Service Layer

### Objectives
- build the provider abstraction
- implement task-based model routing
- keep model selection out of UI components

### Required providers/models
- Gemini 3.1 Pro
- Gemini 3.1 Flash Lite
- Gemini 3 Flash
- MiniMax M-2.7

### Required routing intent
- heavy generation → Gemini 3.1 Pro
- low-latency rewrite → Gemini 3.1 Flash Lite
- fast concept explanation / medium-depth reasoning → Gemini 3 Flash with `thinking_config=medium`
- alternate provider path → MiniMax M-2.7

### Required service contracts
- `generateGuideFromSource(projectId, settings)`
- `summarizeSource(projectId, settings)`
- `explainConceptFromSource(projectId, settings)`
- `designTrainingOutlineFromSource(projectId, settings)`
- `rewriteStep(stepId, operation, context)`
- `explainTerm(term, context, useSearch)`
- `adjustDepth(projectId, mode, newDepth)`
- `validateSafety(stepId, sourceContext)`

### Deliverables
- provider adapter layer
- model routing module
- request/response normalization
- error handling strategy
- local key/settings flow
- retry/fallback rules where sensible

### Done criteria
- UI can call service functions without model-specific logic leaking upward
- provider config is modular
- failures surface clearly
- settings can store local provider configuration

---

## Phase 6 — Output Rendering

### Objectives
- render all four primary output modes cleanly
- preserve structure and editability
- support persona-aware presentation

### Output modes
- Make a Guide
- Summarize
- Explain Concept
- Training Outline

### Deliverables
- structured guide renderer
- summary renderer
- concept renderer
- training outline renderer
- glossary support
- context deck support
- code snippet rendering
- diagram/visual support where appropriate

### Done criteria
- each mode can display structured output
- outputs are readable and stable
- no placeholder-only renderers
- context deck updates appropriately

---

## Phase 7 — Micro-Edit and Safety Tools

### Objectives
- make outputs adjustable after generation
- support low-latency refinements
- support physical workflow safety handling

### Deliverables
- rewrite step actions:
  - Simplify
  - Expand
  - Fix
  - Adjust for skill level
  - Adjust tone/persona
- depth adjustment controls
- explain term actions
- glossary generation helpers
- safety validation service and UI indicators
- unverified warning UI

### Done criteria
- users can refine outputs after generation
- safety ambiguity is surfaced explicitly
- no silent invention of missing safety-critical details

---

## Phase 8 — Audio, Video, and TTS

### Objectives
- support AV-backed projects properly
- connect timestamped steps to media
- provide real TTS behavior

### Deliverables
- audio/video player integration
- timestamp jump behavior
- synced workspace interactions
- guide step narration
- summary narration
- play/pause and progress controls

### TTS direction
- use a desktop-safe TTS strategy
- fallback only if it actually works in the Tauri environment
- no fake playback controls

### Done criteria
- clicking a timestamp jumps correctly
- clicking a step headphone icon produces real audio
- summary narration works
- AV interactions are stable

---

## Phase 9 — Local Exports

### Objectives
- implement required local export paths
- use desktop-appropriate save behavior

### Required exports
**Make a Guide**
- PDF

**Summarize**
- `.md`
- `.txt`
- Copy All

**Explain Concept**
- PDF
- PNG

**Training Outline**
- `.docx`
- `.md`

### Deliverables
- local save dialogs
- export transformers/generators
- format-specific export handlers
- export activity logging

### Done criteria
- required exports save locally
- export files open correctly
- export actions are reflected in activity logs

---

## Phase 10 — Sample Projects

### Objectives
- seed useful demo content
- exercise all core rendering paths

### Required demo projects
- Transmission Valve Body Repair
- Authentication Flow Walkthrough

### Deliverables
- both projects seeded locally
- all four output modes available for both projects
- clear sample project labeling

### Done criteria
- user can open a sample project immediately
- sample projects demonstrate the main product value clearly

---

## Phase 11 — Local Analytics and Review Signals

### Objectives
- support local usage insights
- record quality and error signals without cloud telemetry

### Deliverables
- activity log viewer
- local analytics summaries
- quality flag capture
- error bucket capture

### Metrics
- mode usage
- persona usage
- source type usage
- export volume
- error frequency
- poor-quality flags

### Done criteria
- analytics render from local data
- logs are readable
- flags can be reviewed locally

---

## Phase 12 — QA and Hardening

### Required QA flows

#### Zero-to-Hero Flow
- open app
- create or open project
- import source
- choose persona
- choose mode
- configure settings
- generate output
- verify output appears correctly

#### Recall Flow
- open existing project from library
- review previous outputs
- run a micro-edit
- adjust depth
- verify persistence

#### Export Flow
- export required formats
- confirm files save correctly
- confirm activity log entries

#### AV / TTS Flow
- open AV-backed sample
- click timestamp
- confirm media jump
- click headphone icon
- confirm audible playback

### Hardening tasks
- fix broken imports
- fix dead UI
- fix broken handlers
- verify build
- verify typecheck
- verify lint/tests when configured

### Done criteria
- app builds cleanly
- critical flows function
- no fake core buttons
- no known critical-path breakage remains

---

## Explicit V1 Exclusions

Do not expand scope into:
- cloud accounts
- orgs
- remote sync
- public share links
- billing
- collaboration
- mandatory hosted backend

---

## Final Deliverables

Before calling the project phase complete, ensure the repo contains:

- working Tauri v2 app
- local persistence
- all four output modes
- AI service abstraction
- local exports
- sample projects
- accurate README
- updated spec alignment
- no dead critical-path UI

---

## Build Order Summary

1. scaffold Tauri app
2. build desktop UX shell
3. add SQLite-style local persistence
4. implement input pipeline
5. implement AI service layer
6. render all four modes
7. add micro-edit and safety tools
8. add AV sync and TTS
9. add local exports
10. seed sample projects
11. add local analytics/logs
12. run QA and hardening