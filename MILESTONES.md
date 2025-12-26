## Alignment and MVP Plan

### Guiding Priorities
- Correctness over completeness for MVP: validate one production→delivery chain against the existing Excel before expanding scope.
- Compression is never silent: always warn, require explicit user choice, and log reasons, even when overrides are allowed.

### Milestones for Early Excel Parity
1) **Single-chain parity**: Recreate one production→delivery chain with the same inputs and region calendar; output side-by-side dates vs. Excel plus a per-date reason log.  
2) **Weekend/holiday handling**: Run the same chain through a known holiday week to confirm skips and parity.  
3) **Interactive compression**: Add 0–4 business-day compression with prompts/warnings and guardrails on minimums; validate with one Excel scenario that shortens phases.  
4) **Audit and regression**: Lock in reason codes for every shift and add regression tests covering the above scenarios.

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

