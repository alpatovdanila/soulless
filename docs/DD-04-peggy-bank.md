# DD-04 — Peggy Bank (idle Plinko)

Folder: `games/04-peggy-bank/index.html`. Follow `docs/DD-00-shared-spec.md`.

## Pitch
Balls rain endlessly through a peg board into money buckets. Buy more balls, heavier balls, better buckets. It plays itself — you just make it rain harder. Hypnotic to watch.

## Core loop
- Canvas peg board (staggered peg grid), buckets along the bottom with multipliers (edges high ×, center low — classic plinko payout curve). Balls spawn at top center with slight x jitter, bounce off pegs (simple circle-vs-circle bounce + gravity, hand-rolled), land in a bucket → coins = ballValue × bucketMult, bucket flashes + coin pop + rising "+$N" text.
- Balls auto-respawn: N balls in play at all times (N = balls owned, cap ~120 on screen for perf; beyond cap, extra balls resolve statistically per second — ponytail note it).
- Peg hits: tiny spark + click sound (throttled).

## Progression / addiction
- Upgrades: +1 Ball (cost ×1.25), Ball Value ×1.15/level, Bucket multipliers ×2 per bucket tier upgrade, Drop Rate.
- Golden ball: every 30-60s a golden ball spawns → ×50 payout, sparkle trail, big celebration.
- Milestones at ball counts (10/25/50/100) → new ball skin color + toast.
- Prestige "Melt the bank" at 1e6: convert to permanent ×income tokens.
- Offline earnings modal (expected value per second × time, capped 8h).

## Monetization (per shared spec)
- Rewarded: "▶ Golden rain: 10s of golden balls", "▶ Double offline earnings", "▶ 2x coins 60s".
- Interstitial after each prestige.
- IAP: gems → permanent ×2 ball value, exclusive "Diamond ball" skin, remove ads.

## Tuning
- Start: 1 ball, $1 value, buckets [×10, ×3, ×1, ×0.5, ×1, ×3, ×10]. First 3 min: reach ~8 balls and several value levels.
