## Milestone 2 Testing (API)

Base URL: `https://tv-post-schedule-engine-falling-night-6230.fly.dev`

### What’s included (Milestone 2)
- Unified `/schedule` endpoint with both FORWARD and BACKWARD modes (backward-compatible with the old payload).
- Region-aware calendars and data-driven milestones (template + holiday versions returned in meta).
- Warnings/conflicts and decision log in responses; deterministic `inputHash`.
- Weekend work toggle supported.

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

### What you’ll see
- `schedule`: milestone dates (one row per milestone).
- `warnings` / `conflicts`: empty when all is good; messages if something can’t fit.
- `decisionLog`: notes about the run (e.g., which anchor was used).
- `meta`: includes hashes/versions for traceability.

