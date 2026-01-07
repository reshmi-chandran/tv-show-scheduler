## Milestone 2 & 3 Testing (API)

Base URL: `https://tv-post-schedule-engine-falling-night-6230.fly.dev`

### What’s included (Milestones 2 & 3)
- Unified `/schedule` endpoint with FORWARD and BACKWARD modes (backward-compatible with the old payload).
- Region-aware calendars and data-driven milestones (template + holiday versions returned in meta).
- Warnings/conflicts and decision log in responses; deterministic `inputHash`.
- Weekend work toggle supported.
- Multi-episode scheduling with `episodeCount` and `episodeCadenceDays` (business-day cadence) to ripple schedules across episodes.

### Health check
```
curl https://tv-post-schedule-engine-falling-night-6230.fly.dev/health
```

### Forward scheduling (start from D1)
```
curl -X POST https://tv-post-schedule-engine-falling-night-6230.fly.dev/schedule \
  -H "Content-Type: application/json" \
  -d '{
    "region": "us_west",
    "scheduleMode": "FORWARD",
    "productionStart": "2025-01-06",
    "episodeCount": 1
  }'
```

### Backward scheduling (start from Delivery)
```
curl -X POST https://tv-post-schedule-engine-falling-night-6230.fly.dev/schedule \
  -H "Content-Type: application/json" \
  -d '{
    "region": "us_west",
    "scheduleMode": "BACKWARD",
    "deliveryDate": "2025-04-04",
    "episodeCount": 1
  }'
```

### Multi-episode example (forward, cadence)
```
curl -X POST https://tv-post-schedule-engine-falling-night-6230.fly.dev/schedule \
  -H "Content-Type: application/json" \
  -d '{
    "region": "us_west",
    "scheduleMode": "FORWARD",
    "productionStart": "2025-01-06",
    "episodeCount": 2,
    "episodeCadenceDays": 7
  }'
```
Expect episode 1 D1 on 2025-01-06 and episode 2 D1 offset by 7 business days.

### What you’ll see
- `schedule`: milestone dates (one row per milestone).
- `warnings` / `conflicts`: empty when all is good; messages if something can’t fit.
- `decisionLog`: notes about the run (e.g., which anchor was used).
- `meta`: includes hashes/versions for traceability.

