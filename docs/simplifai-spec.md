# SimplifAI Product Specification

## 1. Product Identity

**Product name:** SimplifAI  
**Application type:** Local-first desktop application  
**Shell:** Tauri v2  
**Frontend:** React + TypeScript  
**Desktop backend:** Rust via `src-tauri`

SimplifAI is a desktop-first AI application designed to turn complex, jargon-heavy, or multi-format source material into clearer, more useful outputs.

The app is built for two broad user types:

- **Physical systems users** such as mechanics, field techs, and shop-floor operators
- **Digital systems users** such as developers, vibe coders, and technical architects

This is **not** a browser-first web app.  
This is **not** a cloud-first SaaS product in v1.  
This is a **local-first Tauri desktop application** with local persistence and local exports.

---

## 2. Core Purpose

SimplifAI converts source material into four primary output modes:

1. **Make a Guide**
2. **Summarize**
3. **Explain Concept**
4. **Training Outline**

The goal is to reduce complexity without flattening useful meaning.

The app must prioritize:

- clarity
- grounded outputs
- practical usability
- low-fluff writing
- safe handling of ambiguous source material
- desktop-first interaction patterns

---

## 3. V1 Product Direction

## V1 is local-first

Version 1 must use:

- local desktop persistence
- local project library
- local exports
- local file import and drag-and-drop
- local project reuse

Version 1 does **not** require:

- cloud accounts
- organizations
- billing
- web-hosted share links
- remote project sync
- multi-user collaboration

Those may be added later, but they are out of scope for this build.

---

## 4. Primary Personas

### 4.1 Physical Systems Persona

**Audience examples:**
- mechanics
- trades workers
- field technicians
- shop supervisors

**Defaults:**
- Mode: **Make a Guide**
- Skill level: **30**
- Depth: **Standard**

**Tone profile:**
- concrete
- literal
- safety-first
- minimal ambiguity
- no unnecessary metaphors in safety-critical content

### 4.2 Digital Systems Persona

**Audience examples:**
- developers
- vibe coders
- software architects
- technical operators

**Defaults:**
- Mode: **Summarize**
- Skill level: **70**
- Depth: **Brief**

**Tone profile:**
- compact
- technical
- high information density
- uses normal dev terminology
- avoids over-explaining

---

## 5. Output Modes

## 5.1 Make a Guide

**Label:** Make a Guide  
**Tagline:** Turn this source into a step-by-step action plan.

Purpose:
- convert source material into ordered, usable, step-by-step guidance

Behavior:
- strictly imperative writing
- concise action-first instructions
- persona-aware tone
- target-language aware
- structured according to the guide schema

The guide should feel like:
- a practical procedure
- a workshop checklist
- a technical setup walkthrough
- a step-by-step operational plan

## 5.2 Summarize

**Label:** Summarize  
**Tagline:** Shrink the source down to the essentials.

Purpose:
- produce an expert-friendly executive digest

Behavior:
- one short context paragraph
- five to ten high-signal bullet points
- suitable for text-to-speech
- concise by default
- approximately 500 words maximum unless the user explicitly adjusts depth

## 5.3 Explain Concept

**Label:** Explain Concept  
**Tagline:** Translate technical jargon into plain English.

Purpose:
- explain terms, concepts, or confusing ideas clearly

Behavior:
- works as a full-document mode
- works as a per-term micro-feature
- supports glossary entries
- supports hover/inline explanation patterns

Style:
- Digital persona may use analogies and memory hooks
- Physical persona should stay more literal and concrete

## 5.4 Training Outline

**Label:** Training Outline  
**Tagline:** Turn the source into a structured lesson plan.

Purpose:
- convert material into teachable structure

Behavior:
- outputs modules, lessons, and learning goals
- up to 5 modules
- each module contains 3 to 5 lessons
- structured, direct, mentor-like tone

---

## 6. Supported Inputs

SimplifAI must support all four output modes across all supported source types.

### Supported file types

**Documents**
- `.pdf`
- `.docx`
- `.txt`
- `.md`

**Images**
- `.png`
- `.jpg`
- `.jpeg`

**Audio**
- `.mp3`
- `.wav`

**Video**
- `.mp4`
- `.mov`

### Hard input caps

**Documents**
- up to 50 MB
- or up to 100 pages

**Video**
- up to 20 minutes
- up to 500 MB

**Audio**
- up to 60 minutes
- up to 50 MB

**Images**
- up to 10 images per project
- each image up to 10 MB

---

## 7. Performance Expectations

**Text and image sources**
- target full run completion within 60 seconds

**Audio and video sources**
- target full run completion within 300 seconds

If processing is expected to take more than 10 seconds, the UI must show a phased progress state instead of a generic spinner.

Example phases:
- Extracting source content
- Analyzing content
- Drafting output
- Finalizing output

---

## 8. Language & Localization

**UI language:** English only for v1

**AI output language:** multilingual

The app must include a **Target Language** control with:
- English
- Source language
- common additional languages such as Spanish, French, and German

Default behavior:
- output in English unless the user selects another target language

All four primary modes must obey the same target language setting.

---

## 9. Tone & Writing Rules

### Global tone
- plain-professional
- direct
- clear
- competent
- minimal fluff

### Hard bans
Generated content must avoid:
- hype language
- corporate buzzword sludge
- emojis
- filler closers
- fake enthusiasm
- empty motivational phrasing

### Persona-specific behavior

**Physical persona**
- concrete
- literal
- safety-focused
- avoid vague instructions in physical workflows

**Digital persona**
- compact
- technical
- efficient
- standard terminology is allowed

### Mode-specific behavior

**Make a Guide**
- imperative and procedural

**Summarize**
- bullet-heavy executive digest

**Explain Concept**
- clearly distinguishes grounded vs external knowledge

**Training Outline**
- structured and mentor-like

---

## 10. Grounding, Safety, and Fallback Rules

### Baseline grounding rule
All outputs are source-faithful by default.

### Per-mode grounding

**Make a Guide**
- may regroup and reorder
- may not invent steps not supported by the source

**Summarize**
- may compress and rephrase
- may not introduce new factual claims

**Explain Concept**
- may use external knowledge when allowed
- must visually distinguish source-grounded explanation from fallback explanation

**Training Outline**
- may add teaching structure
- may not invent source facts

### Physical workflow safety
Implement:

`validateSafety(stepId, sourceContext)`

Behavior:
- detect missing safety-critical details
- do not invent specs such as torque values, voltage ranges, chemical instructions, or other critical numbers if absent from the source
- inject explicit warnings when important source data is missing

Example warning:
- `WARNING: Critical spec missing in source — verify in official documentation.`

Use clear visual indicators such as:
- Unverified
- Needs manual verification
- Source ambiguous

Do not silently smooth over missing safety-critical content.

---

## 11. AI Model Routing

SimplifAI v1 must support explicit model routing through a provider abstraction layer.

The app should be structured so model selection is handled in services, not hard-coded inside UI components.

### Required model set

- **Gemini 3.1 Pro**
- **Gemini 3.1 Flash Lite**
- **Gemini 3 Flash**
- **MiniMax M-2.7**

### Default routing intent

**Heavy generation / deep reasoning**
- use **Gemini 3.1 Pro**

**Low-latency rewrite / micro-edits**
- use **Gemini 3.1 Flash Lite**

**Fast concept explanation / medium-depth reasoning**
- use **Gemini 3 Flash**
- set `thinking_config` to `medium` where applicable

**Alternate reasoning path / optional secondary provider**
- support **MiniMax M-2.7** through the same adapter architecture

### AI architecture requirements
Create a provider/service abstraction that supports:
- model profiles
- task-based routing
- future model swapping
- request logging
- error handling
- retry/fallback behavior where appropriate

API keys should be handled as local user-provided settings, not hard-coded.

---

## 12. Core AI Service Contract

Create a modular AI service layer under `src/services`.

### Main generation functions

- `generateGuideFromSource(projectId, settings)`
- `summarizeSource(projectId, settings)`
- `explainConceptFromSource(projectId, settings)`
- `designTrainingOutlineFromSource(projectId, settings)`

Each function must:
- accept project id and settings
- use the appropriate model profile
- return structured data suitable for rendering

### Micro functions

- `rewriteStep(stepId, operation, context)`
- `explainTerm(term, context, useSearch)`
- `adjustDepth(projectId, mode, newDepth)`
- `validateSafety(stepId, sourceContext)`

### Rewrite operations
`rewriteStep` must support:
- Simplify
- Expand
- Fix
- Adjust for skill level
- Adjust tone/persona

---

## 13. Explain-Term Behavior

Implement:

`explainTerm(term, context, useSearch)`

Behavior:
- if `useSearch = false`, explain using source context only
- if `useSearch = true`, allow external knowledge lookup through the configured AI/service path

Any external explanation must be visually distinct from source-grounded output.

This feature is used by:
- Explain Concept mode
- glossary entries
- hover/inline explanations
- contextual help inside guides

---

## 14. Guide Schema

Guide output must follow a structured schema.

### Guide-level fields
- `title`
- `mission`
- `audience_level`
- `prerequisites`
- `estimated_time`
- `style_variant`

### Style variants
- `workshop_phased`
- `technical_setup`

### Optional phase fields
- `phase_id`
- `phase_title`
- `phase_summary`

### Step fields
- `id`
- `number`
- `title`
- `purpose`
- `instructions[]`
- `code_snippets[]`
- `checks[]`
- `warnings[]`
- `glossary_refs[]`
- `mode`
- `timestamp` or `time_range` for AV-backed sources

### Glossary fields
- `term`
- `definition`
- optional `analogy`

### Wrap-up fields
- `recap`
- `next_steps`

---

## 15. TTS Requirements

TTS must be real.

Fake playback UI is not acceptable.

### TTS behavior

**Guides**
- support step-level playback
- each major step can be played individually
- headphone icon triggers audio for that step

**Summaries**
- support continuous playback

### Controls
- play/pause
- skip forward/back
- progress indication

### Implementation direction
Use a desktop-safe TTS strategy.

Possible paths:
- provider-based TTS if added
- OS/native-capable path
- Web Speech fallback only if it functions reliably inside the Tauri environment

The UI must not pretend TTS works if it does not.

---

## 16. AV Sync Behavior

For audio/video-backed projects:

- clicking a timestamped step jumps the media player to that position
- workspace should keep text and media context linked
- relevant visual or media context should update as the user navigates output

---

## 17. Desktop-First UX Requirements

SimplifAI must behave like a real desktop application.

### Desktop-specific expectations
- native file picker dialogs
- drag-and-drop into the app window
- local project storage
- local source reuse
- local export dialogs
- desktop-safe persistence

Do not design the product as a website that happens to be wrapped later.

---

## 18. Main Views

### 18.1 Unified Drop Zone
First main screen after launch or project creation.

Must include:
- central drag-and-drop area
- supported format messaging
- persona selector
- mode selector
- target language selector
- skill level control
- depth control
- guide style control
- sample project entry points

### 18.2 Intake Flow Panel
Progressive configuration flow:
1. choose persona
2. choose mode
3. adjust settings
4. generate

Controls must visibly reflect live state.

### 18.3 Main Workspace
Sticky split-screen layout:

**Left**
- main output

**Right**
- visual context deck

The right side may include:
- diagrams
- code snippets
- extracted frames
- media player
- supporting visuals
- key structure maps

### 18.4 Focus / Theater Mode
Especially important for guides.

Behavior:
- dim non-essential UI
- emphasize current step
- next/previous step controls
- works well with TTS

### 18.5 Project Library
Must show:
- title
- persona
- modes used
- last updated
- source type

Must support:
- search
- filters
- sample projects

### 18.6 Project Detail View
Must show:
- source files
- outputs by mode
- activity log
- regenerate controls
- micro-edit controls
- safety validation access

### 18.7 Settings View
Must support:
- model/API settings
- local preferences
- theme
- export defaults
- TTS settings if needed

### 18.8 Help / How It Works
Small, concise explanation of:
- upload
- configure
- generate
- refine
- export

---

## 19. Persistence & Storage

Persistence in v1 must be local.

### Required persistence strategy
- local desktop database
- SQLite-style persistence
- stored in an app-managed local location

The app must persist:
- projects
- source file metadata
- output records
- activity logs
- flags
- user preferences
- provider settings metadata where appropriate

This build does not require cloud sync.

---

## 20. Project Model

Each project should include:
- id
- title
- persona
- source files
- outputs by mode
- target language
- timestamps
- metadata
- activity logs

Projects should support:
- re-generation
- depth adjustment
- step rewrites
- safety validation
- local export

---

## 21. Search & Filters

Project Library must support:

### Search
- by title/name

### Filters
- mode
- persona
- source type
- date range

---

## 22. Demo Projects

Seed two built-in sample projects.

### Physical demo
- `Transmission Valve Body Repair`
- video-based or AV-backed

### Digital demo
- `Authentication Flow Walkthrough`
- code/docs-based

Both demo projects must include valid outputs for all four primary modes.

They should be clearly labeled as sample/sandbox projects.

---

## 23. Export Requirements

Version 1 uses local exports only.

### Required exports

**Make a Guide**
- PDF export

**Summarize**
- `.md`
- `.txt`
- Copy All

**Explain Concept**
- single-page PDF
- PNG

**Training Outline**
- `.docx`
- `.md`

Exports must save locally through desktop-appropriate file save behavior.

No cloud share-link system is required in v1.

---

## 24. Error Handling

Required error buckets:
- Upload Error
- Processing Timeout
- Safety Policy

Each error state must:
- explain the issue plainly
- provide Retry
- provide Report Issue

### Bad-but-valid output controls
- Regenerate with more detail
- Regenerate with less detail
- Fix structure

### Quality flags
- one-click poor-quality flag
- optional short comment
- stored locally for analytics and review

---

## 25. Activity Logs & Local Analytics

Each project should maintain a local activity log.

Examples:
- project created
- output generated
- step rewritten
- export created
- validation run

Local analytics should track:
- mode usage
- persona usage
- source-type usage
- export volume
- error bucket frequency
- poor-quality flags

This is local analytics for the app itself, not a cloud telemetry system.

---

## 26. Visual Direction

Use a clean, intentional interface.

### Requirements
- readable
- modern
- practical
- not lifeless
- not over-styled
- desktop-friendly

### Theme support
Implement:
- Dark
- Light
- Comfort

Include a subtle animated background system if it does not reduce readability.

---

## 27. Folder Direction

Expected high-level structure:

- `src/components`
- `src/views`
- `src/services`
- `src/context`
- `src/types`
- `src-tauri`

The exact internal organization can evolve, but the codebase must remain modular and easy to extend.

---

## 28. Non-Negotiable Build Rules

The finished app must:
- build successfully
- have no dead critical-path UI
- have no fake core feature buttons
- support real local persistence
- support real local exports
- support real desktop file handling
- route AI operations through a service abstraction
- preserve clear grounding behavior
- surface safety ambiguity instead of inventing facts

---

## 29. Out of Scope for V1

The following are intentionally excluded from the first build:
- cloud sync
- organizations
- billing
- remote hosting
- social login
- public share links
- multi-user collaboration
- mandatory online-only behavior

---

## 30. Source of Truth

This file is the product source of truth for SimplifAI.

If implementation details are missing, choices should remain consistent with:
- Tauri v2 desktop-first architecture
- local-first behavior
- SQLite-style local persistence
- grounded AI outputs
- safe handling of ambiguous source material
- dual-persona design
- low-fluff, practical UX