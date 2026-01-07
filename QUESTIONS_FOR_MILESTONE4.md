## Questions for Milestone 4 (Compression & Delivery Fit)

Please confirm the following so we can implement compression policies and delivery-fit logic without guesswork:

1) Compression policies
- Do you have named policies (e.g., “standard”, “tight”, “aggressive”)? If yes, please list them.
- For each policy, specify per-phase max compression (business days) and minimum allowed duration.
- Are phase overlaps allowed? If yes, which phases, and by how much?

2) Weekend work
- Should weekend work be counted as:
  - Sat only, or
  - Sat + Sun, or
  - Never?
- Should weekend work be tied to specific policies (e.g., only in “tight”)?

3) Delivery-fit behavior
- If delivery cannot be met within policy limits, should we:
  - Return a BLOCKER with conflicts and suggestions, or
  - Auto-apply the “tightest” policy and report what was done?
- If infeasible even after compression/overlap/weekends, is returning conflicts acceptable?

4) Defaults and priorities
- Default policy to use when none is specified?
- Priority order for compressing phases (which phases to squeeze first)?

5) Overrides
- Do you want per-phase duration overrides and per-episode delivery overrides to remain available in this milestone?

6) Output expectations
- Any specific fields to add to warnings/conflicts/decisionLog for compression (e.g., “Compressed DC from 8bd → 6bd (min=6bd)”)?***

