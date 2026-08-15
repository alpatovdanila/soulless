# DD-05 — Brick Empire (Idle Breakout-like)

Folder: `games/05-brick-empire/index.html`. Follow `docs/DD-00-shared-spec.md`.

## Pitch
Breakout, but you're the bank. Click bricks to crack them yourself, buy balls that bounce forever and do it for you. Clear levels of ever-tougher bricks, earn cash, buy a swarm.

## Core loop
- Canvas field filled with a grid of bricks, each with HP (number shown) and color by HP tier. Click/tap a brick = deal click-damage (crack animation + hit sound). Brick destroyed → cash = its max HP, pop + particles.
- Balls: bounce around the field (straight lines + wall/brick reflection — axis-aligned brick collision, simple), each hit deals its damage. Ball types: Basic (cheap), Splash (damages neighbors), Sniper (targets lowest-HP brick, fast), Gold (damage ×0, cash ×; skip if time — 3 types is enough: Basic/Splash/Sniper).
- Level cleared (all bricks dead) → payout bonus + next level: more bricks, HP ×1.6. "Level N" banner.

## Progression / addiction
- Upgrades: Click damage (×1.5 cost/level), per-ball-type: buy more (cost ×1.35) and upgrade damage (×1.5).
- Always-visible DPS counter and level number. Milestone toast every 5 levels.
- Skip-ahead: if current level takes >90s at current DPS, offer "Skip level" via rewarded ad.
- Prestige at level 25+: reset to level 1 for permanent +10% damage per prestige tier.
- Offline: balls keep earning at 50% rate (computed on load, capped 8h).

## Monetization (per shared spec)
- Rewarded: "▶ 2x damage 60s", "▶ Skip level", "▶ Double offline".
- Interstitial every 10 levels.
- IAP: gems → +1 free Sniper ball, permanent ×2 click damage, remove ads.

## Tuning
- Level 1: 5×4 bricks, HP 1-4, click dmg 1, first Basic ball $50. First 3 min: ~level 5-6, own 3-4 balls.
