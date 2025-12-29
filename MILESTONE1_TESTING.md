## Milestone 1 Testing Guide

### What you can test in Milestone 1 (shippable now)
- Region-aware scheduling: US West / US East / Canada selectable via `region` in the request.
- Holidays and milestones come from data files (no code changes needed to adjust them).
- Endpoints:
  - `GET /` and `GET /health` for quick checks.
  - `POST /schedule` to generate a schedule from D1 and region.
- Live URL: `https://tv-post-schedule-engine-qmq6ta.fly.dev/`.

### How to call the API
Use the deployed base URL `https://tv-post-schedule-engine-qmq6ta.fly.dev/`, or run locally with `npm start`.

**Health checks**
- `GET /` → should return `{ ok: true, message: "tv-post-schedule-engine is running" }`
- `GET /health` → should return `{ status: "healthy" }`

**Schedule generation**
- Endpoint: `POST /schedule`
- Headers: `Content-Type: application/json`
- Body:
  ```json
  {
    "id": "test-episode",
    "d1": "2025-01-06",
    "region": "us_west"
  }
  ```
  - `d1`: ISO date string for D1 (shoot start).
  - `region` (optional): one of `us_west`, `us_east`, `ca`. Defaults to `us_west`.

**Example (curl)**
```bash
curl -X POST https://tv-post-schedule-engine-qmq6ta.fly.dev/schedule \
  -H "Content-Type: application/json" \
  -d '{"id":"m1","d1":"2025-01-06","region":"us_west"}'
```

### What to expect in the response
- Keys for each milestone (D1…D15, LDOD, EC, DC, DC2, PC_START, PC_END, NC1, NC2, LOCK, SP, ONLINE, MX, MX2, MX3, DELIVERY).
- Values are ISO timestamps (date at midnight UTC). Example (abridged):
  - `"D1": "2025-01-06T00:00:00.000Z"`
  - `"D2": "2025-01-07T00:00:00.000Z"`
  - …
  - `"DELIVERY": "2025-04-04T00:00:00.000Z"`

### Data sources (for validation)
- Holidays per region: `data/calendars/*.json`
- Milestone template: `data/milestones.json`

### How to update data (no code change required)
- To change holidays: edit the region file (e.g., `data/calendars/us_west.json`) and redeploy.
- To change milestone offsets/order: edit `data/milestones.json` and redeploy.

### Notes / current limitations
- Holidays are currently the same across regions; structure supports diverging sets.
- No compression loop yet (per LOGIC_DESIGN.md, that’s future scope).
- No explainability metadata yet (reasons for date shifts); future scope.

### Coming next (planned, not in this drop)
- Region calendars diverged per region (real data per region).
- Compression loop and target-delivery fit (per LOGIC_DESIGN.md).
- Explainability metadata (reasons for each date shift).
- Phase templates fully derived from the workbook CSVs, with versioning.
- Back-to-back multi-episode generation and golden tests from the provided CSVs.
- Reloadable configs without redeploy.

