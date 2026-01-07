## Requirements – Single Source of Truth (Milestone View)

### Milestone 1 (Delivered)
- Region-aware scheduling (US West/East/Canada) with holidays from data files.
- Milestone template from data (no hardcoding).
- Forward scheduling via `/schedule` (legacy shape still supported).
- Health endpoints; deployed service.

### Milestone 2 (Delivered)
- Unified `/schedule` with FORWARD and BACKWARD modes (delivery anchoring).
- Region calendars and milestones remain data-driven; meta includes hashes/versions.
- Warnings/conflicts, decision log, deterministic `inputHash`.
- Weekend work toggle supported.
- Full test suite green.

### Milestone 3 (Planned)
- Episode cadence and multi-episode ripple: adjust schedules when D1, delivery, episode count, or phase durations change.
- Cadence support (e.g., 7/10-day or custom per-episode starts).

### Milestone 4 (Planned)
- Compression policies (allowed reductions, overlaps, weekend rules, minimum durations).
- Delivery-fit logic with conflict reporting (no silent fixes).
- More detailed explainability (reasons per shift).

### Milestone 5 (Planned)
- Lightweight UI portal: input form (forward/backward), output grid (episode × milestone), warnings and decision log panels.
- Export (CSV; later Google Sheets format).

### Milestone 6 (Planned)
- Database-backed configuration with an admin UI to edit calendars/templates/policies without redeploy.
- Versioning/audit; file-based configs remain as seed/backup.

### Core Functional Requirements (apply across milestones)
- Scheduling modes: forward (from production/picture start) and backward (from delivery within constraints).
- Calendars/regions: business-day math, region holidays; weekend toggle; clear errors if coverage is insufficient.
- Templates/policies: data-driven phases, durations, min durations, compressibility, dependencies, and compression policies.
- Determinism & explainability: same inputs → same outputs; include inputHash, templateVersion, holidayCalendarVersion; warnings/conflicts with severity; decision log.
- API: `/schedule` supports forward/backward, region, episodeCount, anchors (productionStart/deliveryDate), cadence, allowWeekendWork, template/policy IDs, overrides; health endpoint; clean 400s on invalid input.

### Non-Functional
- Deterministic results; no silent adjustments.
- Data-first; avoid code changes for routine updates.
- Clear error reporting when infeasible.

### Out of Scope (current plan)
- Auth/payments/permissions.
- Complex resource leveling; Gantt/drag-drop UI.
- Third-party integrations and AI suggestions beyond rule-based logic.

