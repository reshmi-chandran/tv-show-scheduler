## Milestones 2, 3, and 5 Testing (UI)

UI Portal: `https://tv-post-schedule-portal.vercel.app/`

Note: Milestone 4 (compression policies/delivery-fit) is skipped for now.

### What to test in the UI
- Mode: Forward or Backward
- Region: US West/East/Canada
- Anchors:
  - Forward: Production Start (D1)
  - Backward: Delivery Date
- Episode Count and Episode Cadence (bd) for multi-episode schedules
- Weekend Work toggle
- Optional production end anchor

### Steps
1) Open the portal and choose mode/region/inputs (D1 or Delivery, episode count, cadence, weekend toggle).
2) Click “Generate” to see the schedule in the grid (multiple pages if many rows).
3) Review Warnings/Conflicts and Decision Log panels.
4) Export CSV if needed.

### What you’ll see
- `schedule`: milestone dates (one row per milestone).
- `warnings` / `conflicts`: empty when all is good; messages if something can’t fit.
- `decisionLog`: notes about the run (e.g., which anchor was used).
- `meta`: includes hashes/versions for traceability.


