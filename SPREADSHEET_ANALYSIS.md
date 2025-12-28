## Spreadsheet Intake Summary (American Love Story workbook)

### Tabs and How They’ll Be Used
- `AtAGlance_Automated`: Episode-level milestone outputs (D1–D15, editor/director/network cuts, lock, finishing/MX, delivery, air). Golden per-episode dates for parity.
- `FullSchedule`: Full grid with day-by-day mappings, transfer section (D1..D15 per episode, cuts, MX, delivery), weekend rows, and holiday markers. Core parity target; reflects ripple logic across episodes.
- `Calendar`: Calendar grid with annotated holidays/hiatus (New Year’s Day, Presidents Day, Good Friday, Memorial Day, Juneteenth, Independence Day, Labor Day, Thanksgiving, Dec 23–Jan 3 hiatus). Baseline holiday set for the business-day engine.
- `Holidays`: Clean holiday list (all/post/shoot) with labels; canonical source for business-day exclusions.
- `Labor`: Prep/Shoot/Post spans and staffing patterns; start/end dates, weeks, holiday counts—useful for validating workday math and staffing durations.
- `Spans`: Per-role prep/shoot/post date ranges and holiday counts; supports verification of business-day calculations.
- `Workdays`: Expanded calendar with weekdays/weekends and holidays; sanity checks for business-day counting.

### Requirements Understanding
- Generate back-to-back episodes from provided starts/durations and produce all post milestones (D1–D15, editor/director/network cuts, lock, finishing/MX, delivery/air).
- Business-day scheduling: skip weekends and holidays/hiatus from `Calendar`, `Holidays`, and `Workdays`.
- Parity: `AtAGlance_Automated` (per-episode outputs) and `FullSchedule` (day-by-day grid/ripple) are the golden sources.
- Compression: user-controlled phase reductions (0–4 business days) with guardrails and warnings; never silent.
- Auditability: every shift must be explainable (holiday/weekend skip, dependency, compression).
- Validation aids: `Labor` and `Spans` provide prep/shoot/post spans and holiday counts to cross-check calculations.

### Milestone Alignment (parity-first)
1) **Single-chain parity**: Use `AtAGlance_Automated` as golden per-episode outputs; cross-check the same episode in `FullSchedule`.
2) **Weekend/holiday handling**: Drive holiday/hiatus from `Holidays`, `Calendar`, and `Workdays`; validate a known holiday week against `FullSchedule`.
3) **Interactive compression**: Apply controlled phase reductions from `AtAGlance` milestones, recompute, and compare to an expected compressed scenario (or defined deltas).
4) **Audit/regression**: Lock reason codes and regression tests using the workbook tabs as golden data for parity, holidays, and compression.

### Next Steps
- Extract one episode (e.g., 101) into a concise JSON golden set (milestone names → dates) for immediate parity tests.
- Normalize holiday set from `Holidays` and ensure the business-day engine matches `Workdays` weekend/holiday skips.
- Confirm any compressed scenarios present in the sheets; if none, define one controlled compression case for validation.

### Example Parity Target (Episode 101, selected milestones)
- Shoot D1–D15: 06/10/2025 through 07/01/2025 (15 shoot days).
- LDOD: 07/02/2025; Editor’s Cut: 07/08/2025; Director’s Cut: 07/14/2025.
- DC2: 07/17/2025; PC Start: 07/18/2025; PC End: 07/28/2025.
- Network Cut 1: 07/29/2025; Network Cut 2: 08/05/2025; Lock: 08/12/2025.
- MX/MX2/MX3: 08/12–08/13/2025; Delivery: 09/09/2025. (Air date TBD in sheet.)

