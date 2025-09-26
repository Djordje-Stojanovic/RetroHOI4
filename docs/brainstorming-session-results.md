Brainstorming Session Results

**Session Date:** 2025-09-26
**Facilitator:** Business Analyst Mary
**Participant:** User

## Executive Summary

**Topic:** RetroHOI4 concept discovery (HOI4 + chess hybrid)

**Session Goals:** Capture MVP pillars, clarify simplifications, and surface risks for a KISS division-level wargame

**Techniques Used:** Mind Mapping; What If Scenarios

**Total Ideas Generated:** 10

### Key Themes Identified:
- Keep the HOI4 operational feel while stripping management layers.
- Rail-based supply and encirclement are the signature mechanics.
- Tech stack must stay code-first, well documented, and hotkey friendly.
- Flexibility for map scale and AI tuning protects long-term depth.

## Technique Sessions

### Mind Mapping - ~20 min
**Description:** Expanded the RetroHOI4 concept into six foundational branches.

#### Ideas Generated:
1. Hourly tick, real-time order execution, duration based on terrain/supply.
2. Division archetypes (Infantry/Armor/Special with light/medium/heavy variants).
3. Supply trace via rail lines and depots, with stockpile penalties.
4. Scenario shell: 10×30 hex map, mirrored capitals, river-crossing rules, victory by holding 3 cities/capital for 7 days.
5. Pure operational decision space—movement, attack, encirclement only.
6. TypeScript + PixiJS stack with HOI4-style UI/UX.

#### Insights Discovered:
- Removing economic layers keeps focus on maneuver chess.
- Supply model supports both simplicity and meaningful encirclements.

#### Notable Connections:
- Tech choice reinforces the keyboard-heavy UX requirement.

### What If Scenarios - ~15 min
**Description:** Stress-tested assumptions via provocative prompts.

#### Ideas Generated:
1. Ensure algorithms scale to larger maps and force counts.
2. Keep reinforcements out of v1; layer them later if desired.
3. Bake AI difficulty sliders in early.
4. Match HOI4 keyboard+mouse workflow across web and desktop builds.

#### Insights Discovered:
- Guardrails for AI aggression will smooth difficulty ramp.
- Flexibility in deployment keeps future scenario design open.

#### Notable Connections:
- Input demands reinforce the need for a robust keybinding system from day one.

## Idea Categorization

### Immediate Opportunities
1. **Core Loop Prototype**
   - Description: Implement hourly tick, order issuing, and movement/combat resolution.
   - Why immediate: Validates the heart of the game.
   - Resources needed: PixiJS project scaffold, unit data schema, tick scheduler.

2. **Supply & Encirclement Validator**
   - Description: Rail trace, stockpile depletion, and encirclement penalties.
   - Why immediate: Encirclement is the signature mechanic.
   - Resources needed: Map graph representation, depot placement logic.

3. **Input Scheme Blueprint**
   - Description: HOI4-like keyboard shortcuts plus mouse interactions.
   - Why immediate: Controls define usability for the solo player.
   - Resources needed: Keybinding map, input dispatcher, UI feedback hooks.

### Future Innovations
1. **Reinforcement & Event Layer**
   - Description: Optional scripted arrivals or replacements.
   - Development needed: Event scheduler, UI messaging.
   - Timeline estimate: Post-MVP once core gameplay is solid.

2. **Expanded Scenario Scale**
   - Description: Larger maps (20×60), additional fronts.
   - Development needed: Performance profiling, AI scaling tests.
   - Timeline estimate: After core systems prove stable.

### Moonshots
1. **Dynamic Campaign Generator**
   - Description: Procedural scenario builder linking historical operations.
   - Transformative potential: Endless replayability with historical flavor.
   - Challenges to overcome: Data authoring pipeline, AI adaptability.

## Insights & Learnings
- Continuous ticks with delayed execution balance micro and macro control.
- Supply simplicity can coexist with historical flavor via rail depots.
- Keyboard parity with HOI4 is non-negotiable for the target experience.
- Scaling considerations must be baked in from the earliest prototypes.

## Action Planning

### #1 Priority: Core Loop Prototype
- Rationale: Everything depends on the feel of movement/combat over time.
- Next steps: Set up PixiJS project, implement tick scheduler, render units on hex grid.
- Resources needed: Hex map assets, unit data schema, time controller.
- Timeline: First development sprint.

### #2 Priority: Supply & Encirclement Validator
- Rationale: Defines RetroHOI4’s unique strategic layer.
- Next steps: Build rail graph, implement supply trace, apply combat penalties.
- Resources needed: Map metadata, pathfinding, combat modifier hooks.
- Timeline: Immediately after core loop basic movement works.

### #3 Priority: Input Scheme Blueprint
- Rationale: Ensures HOI4-style controls across web/desktop from day one.
- Next steps: Define keymap, wire modifiers (ctrl, alt), display command feedback.
- Resources needed: Input manager, UI tooltip system, accessibility checks.
- Timeline: Parallel with early UI work.

## Reflection & Follow-up

### What Worked Well
- Sequential deep dive into each branch clarified MVP boundaries.
- What-if prompts surfaced hidden requirements early.

### Areas for Further Exploration
- Supply combat math: ensure penalties feel fair and intuitive.
- AI behavior profiles: map out how difficulty settings change decision weights.

### Recommended Follow-up Techniques
- Morphological Analysis: explore combat resolution math.
- Question Storming: uncover edge cases for supply/AI interactions.

### Questions That Emerged
- How should difficulty sliders concretely influence AI aggression and caution?
- What visual indicators best communicate supply status without clutter?

### Next Session Planning
- **Suggested topics:** Combat resolution design; AI behavior matrix; UI wireframes.
- **Recommended timeframe:** After core prototype milestone or within two weeks.
- **Preparation needed:** Draft combat formulas, sample AI scripts, rough UI sketches.

---

*Session facilitated using the BMAD-METHODT brainstorming framework*
