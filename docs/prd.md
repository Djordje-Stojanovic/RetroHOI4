# RetroHoi4 Product Requirements Document (PRD)

## Goals and Background Context

### Goals
- Deliver a deterministic hourly tick engine that keeps operational sessions under 45 minutes while preserving Hearts of Iron pacing.
- Provide streamlined division archetypes and rail-based supply mechanics that foreground maneuver, encirclement, and attrition risk.
- Ship a PixiJS-based UI with HOI-style hotkeys so commands register within 100 ms and battlefield state stays legible.
- Keep scope tightly focused on a solo-developer MVP so iteration remains fast and the codebase stays maintainable.

### Background Context
RetroHoi4 targets the gap between Hearts of Iron IV's rich operational warfare and the overhead of its economic and political layers. By concentrating on hourly ticks, division-level orders, and rail-dependent supply, the game recreates the encirclement drama the brief highlights while stripping away distractions that cause scope creep. Building on TypeScript with PixiJS and a local-first stack lets the project owner iterate quickly, wire familiar hotkeys, and keep the simulation transparent enough to support rapid experimentation.

### Change Log
| Date       | Version | Description                                     | Author |
|------------|---------|-------------------------------------------------|--------|
| 2025-09-26 | v0.1    | Initial PRD draft based on project brief review | John   |

## Requirements

### Functional
1. FR1: The simulation engine must apply all queued division orders on every in-game hour using a deterministic scheduler that can execute at least 100 consecutive ticks without state divergence or runtime errors.
2. FR2: The system must support a library of division archetypes (light/medium/heavy infantry and armor) whose stats and behavior can be configured via JSON data files and referenced by scenarios at load time.
3. FR3: Rail-based supply tracing must calculate stockpile flow, detect cut rail connections, and apply attrition/penalty modifiers to encircled divisions within one tick of a supply break.
4. FR4: The PixiJS tactical UI must allow the player to issue movement and combat orders, visualize supply lines/encirclements, and mirror core HOI-style hotkeys with on-screen feedback confirming commands.
5. FR5: A scenario loader must initialize mirrored 10-30 hex battlefields, starting forces, and scripted supply depots so that the MVP can run complete operational sessions end-to-end.

### Non Functional
1. NFR1: Command inputs should reflect on screen within 100 ms on the target Windows 11 development machine to maintain flow.
2. NFR2: Hourly tick resolution must complete in under 50 ms for scenarios with up to 100 units and 30 hexes to sustain 60 FPS rendering.
3. NFR3: The Electron build must launch and run without dependency changes on a Windows 11 desktop, aligning with the solo-developer environment.
4. NFR4: Core simulation features (tick scheduling, supply tracing, encirclement penalties) must have automated test coverage that can execute locally in under 2 minutes to support rapid iteration.
5. NFR5: Project source must follow the planned monorepo layout (src/core, src/ui, src/assets, tests) with modular boundaries that keep rendering, simulation, and data isolated for maintainability.

## User Interface Design Goals

### Overall UX Vision
Deliver a fast, information-dense operational map where I can read supply, orders, and combat states at a glance without modal clutter. The interface should feel like HOI4 stripped to its essentials: hex map central, hotkeys primary, overlays and tooltips offering depth only when needed.

### Key Interaction Paradigms
- Keyboard-first control scheme mirroring HOI4 hotkeys, with click-to-queue as a fallback.
- Hover-driven tooltips revealing unit stats, supply status, and projected outcomes so the main map stays clean.
- Layered overlays (supply, rail lines, encirclement risk) toggled via hotkeys for instant situational awareness.
- Hourly tick timeline strip displaying upcoming orders and resolution logs without stealing focus.

### Core Screens and Views
- Operational Hex Map with unit counters, supply overlays, and combat indicators.
- Order Planning Sidebar for queueing movements, attacks, and supply toggles.
- Hourly Tick Timeline overlay summarizing recent and upcoming events.
- Scenario Setup Dialog for selecting mirrored maps, starting forces, and supply depots.

### Accessibility: None
_Assumption: single-user hobby project, no formal accessibility target yet._

### Branding
Muted military palette with parchment-style UI chrome and HOI4-inspired typography; subtle "retro war room" motifs to reinforce theme without distracting animations.

### Target Device and Platforms: Desktop Only
Primary experience is the Windows 11 Electron build with optional same-machine browser preview; interactions and layouts are optimized for large monitors and mouse/keyboard input.

## Technical Assumptions

### Repository Structure: Monorepo
Single repository containing src/core, src/ui, src/assets, and tests, plus build/config scripts for the Electron shell and tooling.

### Service Architecture
Single-process client application where the simulation engine, PixiJS renderer, and UI overlays run inside one Electron main/renderer combo; no backend services or remote APIs.

### Testing Requirements
Prioritize automated unit tests for simulation subsystems (tick scheduler, supply tracing) plus targeted integration tests covering command dispatch to rendered outcomes; manual scenario run-throughs retained for UX polish.

### Additional Technical Assumptions and Requests
- Primary language is TypeScript; build and bundling handled via Vite (or equivalent) targeting both browser preview and Electron.
- Rendering implemented with PixiJS; React-lite overlay optional but only if it does not slow hourly tick loop.
- Scenario data, unit templates, and keybindings stored in JSON files under src/assets/data.
- Electron packaging aimed at Windows 11 desktop; no macOS/Linux support required.
- Optional browser preview served via npm run dev for rapid iteration, but desktop build remains canonical.
- No external network dependencies; all assets and code must run offline.

## Epic List

1. Epic 1: Foundation & Tick Prototype – Scaffold the TypeScript/PixiJS/Electron workspace, wire the deterministic hourly tick loop, and deliver a canary scenario to validate build, input, and simulation plumbing.
2. Epic 2: Division Archetypes & Combat Resolution – Implement configurable division templates, movement/combat resolution, and validation harnesses so engagements feel like HOI maneuver chess.
3. Epic 3: Rail Supply & Encirclement Pressure – Build rail tracing, depot stockpiles, and attrition penalties that react within one tick, making supply gambles and encirclements decisive.
4. Epic 4: Command UI & Scenario Delivery – Layer the PixiJS interface, hotkey-driven commands, overlays for supply and combat state, and ship a 10-30 hex mirrored scenario playable end-to-end.

## Epic 1 Foundation & Tick Prototype

**Expanded Goal**  
Lay down the RetroHoi4 codebase, toolchain, and first playable loop so every later feature sits on a reliable tick engine. By the end of this epic the project builds, runs, and proves the deterministic hourly scheduler on a lightweight canary scenario.

### Story 1.1 Project Scaffold and Tooling
As a solo developer, I want a TypeScript/PixiJS/Electron workspace scaffolded with linting and scripts, so that I can iterate quickly without wrestling project setup.

**Acceptance Criteria**
1. A fresh clone installs with npm install and exposes scripts for dev (browser preview), electron:dev, build, and test.
2. Repository contains src/core, src/ui, src/assets, tests, and configuration for Vite (or equivalent), TypeScript, ESLint, and Prettier.
3. Running npm run electron:dev opens the Electron shell displaying a placeholder map canvas and tick counter.
4. Running npm run test executes a placeholder unit test to confirm Jest/Vitest wiring.

### Story 1.2 Deterministic Hourly Tick Engine
As a player, I want the simulation to process hourly ticks deterministically, so that battles play out consistently and can be validated.  
_Prerequisite: Story 1.1_

**Acceptance Criteria**
1. Core tick scheduler processes registered systems every in-game hour with deterministic ordering.
2. A scripted test scenario runs 100 ticks without divergence, asserting identical state across two runs.
3. Unit tests cover tick registration, ordering, and deterministic replay checks.
4. Console/debug overlay displays current hour and queued events during the demo scenario.

### Story 1.3 Canary Scenario Rendering and Hotkeys
As a player, I want to issue simple move orders via HOI-style hotkeys and see units update each tick, so that the loop feels interactive from the start.  
_Prerequisite: Stories 1.1, 1.2_

**Acceptance Criteria**
1. Minimal unit counters render on the PixiJS map and update position each tick according to queued orders.
2. Keyboard shortcuts (e.g., M for move) queue orders that execute on the next tick with visible feedback.
3. The canary scenario logs order execution and results to a timeline/debug panel.
4. QA checklist confirms hotkeys respond within the 100 ms responsiveness target on the dev machine.

## Epic 2 Division Archetypes & Combat Resolution

**Expanded Goal**  
Introduce configurable division templates and implement movement/combat rules so engagements feel like HOI maneuver chess. This epic turns the tick skeleton into battles with meaningful unit differentiation.

### Story 2.1 Division Archetype Data Model
As a scenario designer, I want to define division archetypes via JSON, so that scenarios can mix light/medium/heavy infantry and armor without code changes.  
_Prerequisite: Stories 1.1–1.3_

**Acceptance Criteria**
1. JSON schema supports archetype stats (movement, attack, defense, supply draw) and is validated on load.
2. Archetype loader populates simulation entities without blocking the tick loop.
3. Unit tests verify sample archetypes deserialize correctly and reject invalid data.
4. Demo scenario shows mixed archetypes with distinct stats logged each tick.

### Story 2.2 Movement and Combat Resolution
As a player, I want divisions to resolve movement and combat outcomes based on archetype stats, so that maneuvers produce believable results.  
_Prerequisite: Story 2.1_

**Acceptance Criteria**
1. Movement rules account for terrain cost and enforce one-hex-per-hour limits per archetype.
2. Combat resolver compares attacker/defender stats, produces casualties, and updates control of contested hexes.
3. Deterministic tests cover combat edge cases including multi-hour engagements and equal-strength stalemates.
4. Timeline/debug panel logs combat summaries with attacker/defender archetype references.

### Story 2.3 Combat Outcome Validation Harness
As the developer, I want an automated harness that replays scripted combats, so that regressions surface immediately.
_Prerequisite: Stories 2.1, 2.2_

**Acceptance Criteria**
1. Harness loads scenario snippets, runs tick sequences, and asserts expected control, casualties, and unit states.
2. CI task executes harness under npm run test within the two-minute budget.
3. Failure output includes diff of expected vs. actual results for quick diagnosis.
4. Documentation in tests/README.md explains how to add new combat scenarios.

## Epic 3 Rail Supply & Encirclement Pressure

**Expanded Goal**  
Model rail-based logistics so cutting lines matters instantly. Supply tracing, depots, and attrition penalties deliver the encirclement tension promised in the brief without over-scoping economy systems.

### Story 3.1 Rail Network Representation
As a scenario designer, I want to define rail lines, depots, and connections, so that supply tracing can follow explicit infrastructure.  
_Prerequisite: Epic 1 and Epic 2 stories_

**Acceptance Criteria**
1. JSON definitions capture rail segments, depots, capacities, and initial stockpiles.
2. Loader builds a graph structure accessible to the simulation each tick.
3. Unit tests verify graph creation, bidirectional traversal, and error handling for broken links.
4. Debug overlay can visualize rails and depots for developer inspection.

### Story 3.2 Supply Tracing and Stockpile Consumption
As a player, I want divisions to pull supply along rails and suffer when cut off, so that encirclements matter strategically.  
_Prerequisite: Story 3.1_

**Acceptance Criteria**
1. Supply algorithm runs each tick, tracing from depots to divisions and decrementing stockpiles.
2. Encircled units receive attrition, combat penalties, and a status flag within one tick of disconnection.
3. Tests cover full supply, partial throughput, and complete cutoff scenarios.
4. Timeline/debug output notes supply shortfalls and attrition events.

### Story 3.3 Supply Validation Scenarios
As the developer, I want scripted supply test cases to prevent regressions, so that maintenance stays manageable.  
_Prerequisite: Stories 3.1, 3.2_

**Acceptance Criteria**
1. Harness scenarios assert supply delivery timelines, attrition onset, and recovery when rails reopen.
2. CI ensures supply tests run alongside combat harness under the two-minute window.
3. Failure logs include highlighted rail segments causing the break.
4. Documentation explains how to craft new supply-focused fixtures.

## Epic 4 Command UI & Scenario Delivery

**Expanded Goal**  
Wrap the simulation in a responsive UI that mirrors HOI muscle memory and deliver a polished MVP scenario. This epic turns the engine into a game-ready experience.

### Story 4.1 Command Interface and Hotkey Layer
As a player, I want to issue and review orders through an intuitive command layer, so that managing divisions stays quick and feedback-rich.  
_Prerequisite: Epics 1-3 stories_

**Acceptance Criteria**
1. Command sidebar lists selected units, queued orders, and allows drag/drop reordering.
2. Hotkey map covers movement, attack, hold, supply toggles, and includes on-screen confirmations.
3. UI responds within the 100 ms responsiveness target measured on dev hardware.
4. QA checklist passes accessibility sanity (focus order, readable contrasts) even though formal targets are deferred.

### Story 4.2 Overlays and Timeline Visualization
As a player, I want map overlays and a tick timeline, so that I can read supply state, encirclement risk, and recent events without digging.  
_Prerequisite: Story 4.1_

**Acceptance Criteria**
1. Toggleable overlays display rail supply paths, encircled units, and combat hotspots.
2. Timeline visual highlights last 24 in-game hours with filterable combat/supply entries.
3. Performance profiling shows overlays do not break the 60 FPS goal on dev machine.
4. Manual playtest script confirms overlays aid decision-making in the MVP scenario.

### Story 4.3 MVP Scenario Packaging
As the product owner, I want a packaged mirrored scenario that showcases the full loop, so that the MVP is playable end-to-end.  
_Prerequisite: Stories 4.1, 4.2_

**Acceptance Criteria**
1. Scenario config includes 10-30 hex map, mirrored forces, rail network, and scripted supply depots.
2. README walkthrough explains setup, controls, and victory conditions.
3. Build pipeline produces an Electron distributable installer/zip for Windows 11.
4. Final smoke test completes a session in under 45 minutes and records metrics for tick stability, input responsiveness, and supply integrity.

## Checklist Results Report
**Executive Summary**
- Overall PRD completeness: ~90%
- MVP scope appropriateness: Just Right
- Readiness for architecture: Ready
- Critical gaps: Document solo-play feedback loop and minimal accessibility notes for future reference.

**Category Analysis**
| Category | Status | Critical Issues |
| --- | --- | --- |
| 1. Problem Definition & Context | PASS | Baseline metrics can be added later if needed. |
| 2. MVP Scope Definition | PASS | — |
| 3. User Experience Requirements | PARTIAL | Document minimal accessibility stance and note how error states will evolve. |
| 4. Functional Requirements | PASS | — |
| 5. Non-Functional Requirements | PASS | — |
| 6. Epic & Story Structure | PASS | — |
| 7. Technical Guidance | PARTIAL | Capture lightweight mitigation plan for AI complexity when that work begins. |
| 8. Cross-Functional Requirements | PARTIAL | Explicitly state deployment cadence/support expectations. |
| 9. Clarity & Communication | PASS | — |

**Top Issues by Priority**
- BLOCKERS: None.
- HIGH: None.
- MEDIUM: Document self-play testing as the feedback loop, note minimal accessibility assumptions, and add planned AI risk follow-up.
- LOW: Consider logging overlay priorities and UI microcopy guidance when convenient.

**MVP Scope Assessment**
- Scope feels Just Right for the MVP; no cuts needed.
- No missing essentials identified.
- Complexity stays manageable for a solo developer.
- Timeline target (~3 months) still realistic.

**Technical Readiness**
- Stack and deployment constraints are clear.
- Key technical risks: future AI behaviours and UI readability as scope grows.
- Architect should plan future investigation points for AI strategy and UI feedback readability.

**Recommendations**
- Add a short note in the PRD describing the self-playtest feedback loop and minimal accessibility stance.
- When AI work kicks off, draft a lightweight mitigation plan for complexity creep.
- Record the intended deployment cadence (e.g., after each epic) for personal tracking.
- Optionally sketch overlay priority/microcopy guidance once UI work begins.

## Next Steps
### UX Expert Prompt
Use `docs/prd.md` to outline UI overlays, hotkey confirmations, and timeline visuals that keep the operational map readable for fast solo play.

### Architect Prompt
Using `docs/prd.md`, design the TypeScript + PixiJS/Electron architecture: define module boundaries (`src/core`, `src/ui`, `src/assets`, `tests`), tick/supply services, and packaging workflow for Windows 11.
