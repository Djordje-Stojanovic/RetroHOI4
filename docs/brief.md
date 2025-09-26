# Project Brief: RetroHoi4

## Introduction
RetroHoi4 distills Hearts of Iron IV’s operational warfare into a streamlined, fast-moving chess-like experience focused on maneuver, supply, and encirclement. This brief captures the vision, target users, MVP scope, and phased roadmap so the project can stay KISS while moving quickly from concept to implementation.

## Executive Summary
RetroHoi4 is my personal experiment in boiling Hearts of Iron IV down to the pure maneuver warfare I love. The game runs on hourly ticks, division-level orders, and a rail-based supply system that rewards clean encirclements. I’m building it for myself—fast, simple, and without the grand-strategy clutter—so every session feels like HOI4 chess on a custom PixiJS/TypeScript board.

## Problem Statement
HOI4’s grand-strategy layers make it tough to enjoy pure operational maneuvering without diving into production, politics, and research. Large-scale management bogs down quick experiments, and even solo sessions demand constant attention to logistics menus instead of the front line. Existing wargames that strip things back rarely deliver HOI4’s hourly pacing, encirclement drama, or the familiar controls I enjoy. I need a personal sandbox that lets me issue orders, watch hourly combat unfold, and feel the tension of rail-bound supply without all the meta overhead.

## Proposed Solution
RetroHoi4 keeps HOI4’s hourly heartbeat but turns everything else into a clean tactical puzzle. I issue division-level orders on a small hex map, watch them resolve over a tick-based scheduler, and see supply flow along rails I can actually reason about. Encirclements matter because stockpiles drain and penalties bite, yet the rules stay light enough that I never leave the front. Building in TypeScript with PixiJS keeps iteration fast, lets me wire HOI4-style hotkeys directly, and gives me a foundation I can tweak whenever new ideas hit. It’s HOI maneuver chess tuned for one obsessed player—me.

## Target Users
### Primary User Segment: Solo HOI Veteran (Me)
- Demographic snapshot: PC strategy gamer, long-time Hearts of Iron IV player comfortable with keyboard-first interfaces and mod tinkering.
- Current workflow: Plays HOI4 single-player campaigns, experiments with focus trees, and frequently pauses to manage production, diplomacy, research, and supply screens.
- Needs & pain points: Wants the encirclement rush and operational maneuvering without slogging through economic micromanagement, menu juggling, or waiting on day-long ticks.
- Goals: Rapid-fire operational sessions where division movements, supply gambles, and front-line decisions resolve in minutes, letting me iterate on tactics and enjoy the chess-like puzzle on my own terms.

## Goals & Success Metrics
### Business Objectives
- Ship a playable RetroHoi4 MVP to myself within three months so I can run full battles start-to-finish on weeknights.
- Keep project scope tight enough to avoid burnout—no more than two simultaneous feature tracks at any point.

### User Success Metrics
- Finish a 10-hex scenario in under 45 minutes while staying in the flow (no long pauses or boredom).
- Pull off at least one satisfying rail-based encirclement per session without fighting the UI.

### Key Performance Indicators (KPIs)
- **Tick Stability:** Hourly simulation resolves without errors across 100 consecutive ticks.
- **Input Responsiveness:** Commands reflect on-screen within 100 ms on my dev machine.
- **Supply Integrity:** Encirclement penalties trigger correctly in 95% of test cases I script.

## MVP Scope
### Core Features (Must Have)
- **Hourly Tick Engine:** Deterministic scheduler that applies movement/combat orders each in-game hour so I can feel the HOI rhythm without day-long waits.
- **Division Archetype System:** Light/medium/heavy infantry and armor templates with simple stats to let me test maneuver matchups fast.
- **Rail Supply & Encirclement Rules:** Supply traced via rail lines, depots, and stockpiles; cut rails trigger attrition penalties that make encirclement the star.
- **PixiJS Hex Map & UI:** Basic hex renderer, unit counters, and HOI-style hotkeys/tooltips so commanding divisions stays second nature.

### Out of Scope for MVP
- Diplomatic, political, or economic subsystems.
- Reinforcements/events beyond scripted starting forces.
- Multiplayer or networked features.
- Detailed production chains, tech trees, or research screens.

### MVP Success Criteria
I can set up a mirrored 10–30 hex scenario, issue orders over a week-long timeline, watch supply swings decide outcomes, and feel confident enough in the codebase to iterate without dreading refactors.

## Post-MVP Vision
### Phase 2 Features
Add AI aggression sliders and behavior profiles so I can dial challenge levels without rewriting the core loop; experiment with optional daily summary overlays that capture supply swings without adding management overhead.

### Long-term Vision
Grow RetroHoi4 into a personal campaign lab where I replay handcrafted operations, test alternate histories, and tweak AI habits or supply parameters to keep scenarios fresh.

### Expansion Opportunities
Build a lightweight scenario editor and mod hooks so I can swap maps, unit stats, or supply rules on the fly; experiment with larger (20–60 hex) battlefields once performance and AI scale cleanly.

## Technical Considerations
### Platform Requirements
- **Target Platforms:** Windows desktop build via Electron plus browser preview for quick tests.
- **Browser/OS Support:** Focus on latest Chrome/Edge; keep Electron bundle working on Windows 11 (no macOS/Linux work planned).
- **Performance Requirements:** Sustain smooth 60 FPS on my dev PC (mid-tier GPU) while stepping through 100 units and 30 hexes; hourly ticks should settle in under 50 ms.

### Technology Preferences
- **Frontend:** TypeScript + PixiJS for rendering; React-lite UI overlays if needed.
- **Backend:** Pure client-side simulation with Node/TypeScript tooling for build scripts.
- **Database:** JSON/flat-file data for unit stats, scenarios, and keybindings.
- **Hosting/Infrastructure:** Local builds only; optional GitHub releases for self-updates.

### Architecture Considerations
- **Repository Structure:** Monorepo with `src/core`, `src/ui`, `src/assets`, and `tests` to keep logic separated from rendering.
- **Service Architecture:** Modular simulation engine (tick scheduler, logistics, combat) wrapped by a command dispatcher; minimal services beyond the main loop.
- **Integration Requirements:** Potential future hooks for map editors or AI trainers but no dependencies now.
- **Security/Compliance:** None beyond keeping dependencies current; private project so no auth layers.

## Constraints & Assumptions
### Constraints
- **Budget:** Personal time and hobby funds only—no external spend or monetization targets.
- **Timeline:** Soft three-month MVP aim, but pace flexes with my free time; no hard deadlines.
- **Resources:** Solo developer (me) wearing design, engineering, and QA hats; minimal opportunity for parallel tracks.
- **Technical:** Commit to TypeScript + PixiJS even if other engines tempt me; no third-party backend or heavyweight frameworks.

### Key Assumptions
- I’ll stay motivated by keeping scope tight and visible progress happening weekly.
- My Windows dev machine comfortably handles the Electron build and PixiJS rendering targets.
- Existing HOI4 muscle memory translates well to my custom keybinding scheme, reducing UX iteration needs.
- Light-weight JSON data files remain sufficient for unit/scenario definition without needing databases.

## Risks & Open Questions
### Key Risks
- **Scope Creep:** Enthusiasm might tempt me to bolt on economy or reinforcements, derailing the MVP timeline.
- **AI Complexity:** Building an enemy AI that feels smart but stays simple could burn time without solid results.
- **UI Overload:** Even without grand strategy menus, poor feedback could make supply or encirclement states hard to read.

### Open Questions
- How granular should combat resolution be to stay readable yet interesting?
- What visual cues best communicate rail supply status and encirclements at a glance?

### Areas Needing Further Research
- Reference minimalist wargames for combat math that balances clarity and depth.
- Investigate PixiJS techniques for layering UI feedback (supply overlays, order indicators).

## Appendices
### A. Research Summary
- Brainstorming session (2025-09-26) highlighted hourly ticks, division archetypes, rail supply, HOI-style controls, and anti-scope principles.
- What-if prompts reinforced AI difficulty sliders, supply clarity, and scaling considerations for larger maps.
- Immediate action priorities established: core loop prototype, supply/encirclement validator, input scheme blueprint.

### B. Stakeholder Input
_Not applicable—solo project._

### C. References
- `docs/brainstorming-session-results.md` – RetroHoi4 concept brainstorming (session notes, priorities, open questions).

## Next Steps
### Immediate Actions
1. Scaffold the PixiJS + TypeScript project with core directories (`src/core`, `src/ui`, `tests`) and an hourly tick prototype.
2. Implement rail supply tracing on a small mirrored map and script test cases to validate encirclement penalties.
3. Draft the HOI4-inspired keybinding map and hook it into the command dispatcher for core orders.

### PM Handoff
This Project Brief provides the full context for RetroHoi4. Please start in “PRD Generation Mode,” review the brief thoroughly, and work with me section by section to build the PRD following its template—asking for clarification or proposing improvements as needed.
