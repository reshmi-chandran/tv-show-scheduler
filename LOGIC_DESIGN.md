## Post-Production Scheduler Logic Design

Detailed logic to replace the Excel-based full-season post-production schedule generator with a rules-driven web application.

### Objectives
- Generate back-to-back episodes from production start dates and episode lengths.
- Ripple all post milestones: editor cuts, director cuts, network reviews, lock, finishing, delivery.
- Enforce business-day calculations, skipping weekends and region-specific union holidays (US East, US West, Canada).
- Support user-controlled compression of specific phases by 0–4 business days when production-to-delivery timelines don’t fit.
- Maintain explainability: every date should be traceable to its rule and inputs.

### Quick Narrative Example (plain English)
- **Inputs**: Region US West calendar; Production start = Mon Jan 6; Episode length drives default edit durations; Target delivery = Fri Mar 14.
- **Propagation**: Editor cut starts Jan 6 → ends Fri Jan 17 (10bd). Director cut starts Mon Jan 20 → ends Fri Jan 24. Network review starts Mon Jan 27 → ends Wed Jan 29. Lock Thu Jan 30 → Fri Jan 31. Finishing starts Mon Feb 3 → ends Mon Feb 10. Delivery Tue Feb 11.
- **Holiday handling**: If US West holiday on Mon Feb 3, finishing shifts to Tue Feb 4; delivery moves to Thu Feb 13, with a reason code.
- **Compression scenario**: Need delivery Fri Feb 7 (pull in 4bd). Apply safe compression: Editor cut -2bd, Director cut -1bd, Finishing -1bd (all within 0–4bd and above minimums). Recompute → delivery Fri Feb 7; reasons logged.

### Core Concepts
- **Episode template**: Defines ordered phases with default durations and dependencies.
- **Calendars**: Business-day source per region; allow overrides at show or episode level.
- **Dependency graph**: Directed graph of phases (nodes) with finish-to-start (or start-to-start if needed) edges.
- **Compression rules**: Per-phase min duration and allowed reduction (0–4 business days).
- **Propagation**: Forward-pass scheduling that respects dependencies and calendars, with recompute on any input change.

### Calendar & Business-Day Engine
- Maintain holiday sets for US East, US West, Canada; support custom additions.
- Business-day addition/subtraction: date_shift(start, days, calendar) that skips weekends/holidays and is deterministic.
- Normalize to timezone (e.g., show-level TZ) to avoid DST boundary surprises; treat all calculations as local dates without times.

### Episode Generation
1) Input: production start date, episode lengths (e.g., runtime or phase-driven).
2) Create episode shells back-to-back:
   - Episode `n` start = `delivery` of episode `n-1` (or specified gap rule).
   - If using runtime to derive edit length, map runtime → default edit durations via configurable table.
3) Instantiate phase nodes from the template for each episode.

### Milestone Propagation
- For each episode, perform a topological traversal of the phase graph:
  - Compute earliest start based on predecessors’ finishes and any mandated gaps.
  - Compute finish = date_shift(start, duration, calendar).
- Respect per-episode calendar selection; if not set, inherit show-level calendar.
- Store lineage: which rule and predecessor produced each date.

### Compression Logic
- Compression input: user selects phases and reduction (0–4 business days) respecting per-phase max reduction and minimum duration.
- Simulation-first: calculate a compressed plan without mutating the baseline; surface impact deltas.
- Apply compression by shortening the phase duration; re-run propagation; reject if any phase goes below minimum or creates negative slack.
- Present explanations: “Director cut compressed by 2 days to meet delivery; weekend and CA holiday skipped.”

### Detecting Schedule Misfit
- Define target delivery per episode or season.
- After propagation, compare computed delivery to target:
  - If delivery > target, compute required delta.
  - Suggest feasible compressions within allowed ranges; if insufficient, flag as infeasible and require date move or scope change.
- Highlight constraints that block fit (e.g., min durations, fixed review windows).

### Conflict Handling & Guardrails
- Prevent overlaps where rules forbid them (e.g., one editor cannot cut two episodes simultaneously if resource limits are modeled).
- Clamp durations to minimums; reject zero or negative spans.
- Validate calendars exist for chosen region; fallback is explicit, never silent.
- Detect weekend/holiday-only windows and push to next business day with a reason code.

### Data Model (conceptual)
- `Show`: region calendar, default template version, timezone.
- `Episode`: number, production start, target delivery, selected calendar (optional override).
- `PhaseTemplate`: name, default duration, min duration, max compression, dependencies.
- `PhaseInstance`: episode id, template ref, start date, finish date, compression applied, reasons.
- `Calendar`: region, holiday set, custom overrides.

### Explainability & Audit
- Persist reasons for every computed shift (weekend skip, holiday skip, dependency finish, compression).
- Keep both baseline and last-computed compressed plan for comparison.
- Provide diff: before/after dates per phase.

### Testing Strategy
- Unit tests for business-day arithmetic across weekends, region holidays, DST boundaries.
- Scenario tests mirroring the Excel outputs (golden cases).
- Property tests for compression bounds (never below min, never negative durations).
- Regression tests for cross-episode ripple effects when upstream dates change.

### Extensibility
- Calendar provider abstraction to swap holiday sources.
- Template versioning so new rules don’t retroactively alter locked episodes.
- Hooks for resource constraints if needed later (editor/finishing bay availability).

### Minimal UI Expectations (logic-first)
- Grid/list showing per-episode milestones with reason tooltips.
- Controls to adjust compression per phase with simulated results before applying.
- Indicators when schedule doesn’t fit and guided suggestions to resolve.

