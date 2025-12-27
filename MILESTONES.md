## Alignment and MVP Plan

### Milestones for Early Excel Parity (with exit criteria)
1) **Single-chain parity**
   - Goal: One production→delivery chain matches Excel dates for the same inputs and calendar.
   - Exit: Side-by-side dates match; per-date reason log produced (CSV/JSON).
2) **Weekend/holiday handling**
   - Goal: Same chain through a known holiday week correctly skips weekends/holidays.
   - Exit: Parity confirmed on holiday scenario; reason log shows skips.
3) **Interactive compression**
   - Goal: 0–4 business-day compression per phase with prompts/warnings and min-duration guardrails.
   - Exit: Excel scenario with shortened phases matches; UI/API enforces explicit user choice.
4) **Audit and regression**
   - Goal: Reason codes for every shift; regression coverage for the above scenarios.
   - Exit: Audit trail stored; automated tests green on parity, holidays, and compression cases.

### Saved Projects and Reopening
- Persist a “Schedule Project” with: inputs (production start, episode lengths, target delivery), region calendar + overrides, template version, computed milestones, compression choices, and the reason/audit log.  
- On reopen: load the saved snapshot; re-run computation only if inputs/template/calendar version changed; always show last-computed dates plus an explicit “recompute” option.  
- Keep both baseline (uncompressed) and last compressed plans so users can compare and roll back.

### MVP Timeline (assumes API + minimal UI, one region set, one chain)
- **Week 1**: Intake Excel rules; data model; calendar/holiday sets; business-day calculator; dependency graph scaffold.  
- **Week 2**: Single-chain propagation matching Excel; side-by-side diff tool; weekend/holiday skips; unit tests on date math.  
- **Week 3**: Compression flow with explicit prompts/warnings; guardrails on minimum durations; audit trail for every shift.  
- **Week 4**: Persistence for “Schedule Project”; load/reopen behavior; regression runs against Excel golden cases.  

Adjustable based on your stack, access to the Excel workbook, and any existing services we can reuse.

