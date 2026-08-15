# DD-01 — Cosmic Crush (Suika-like physics merge)

Folder: `games/01-cosmic-crush/index.html`. Follow `docs/DD-00-shared-spec.md`.

## Pitch
Drop space rocks into a jar. Two identical bodies touching merge into the next bigger one: Dust → Pebble → Asteroid → Comet → Moon → Planet → Gas Giant → Star → SUN. Overflow the jar = game over. Chase high score + the mythical Sun.

## Core loop
- Tap/click to drop the current body at cursor/finger x. Next-piece preview shown.
- Simple 2D circle physics: gravity, restitution ~0.2, circle-circle collision with positional correction and impulse. Write it by hand (~80 lines) — no physics lib. Bodies are circles with radius by tier; sleep distant-stable bodies if perf needs it.
- Merge: two same-tier circles in contact → new body at midpoint, next tier, score += tier value, chain combos multiply score. Merge = particle burst + pop sound + slight shake scaled by tier.
- Danger line at top; crossing it for >2s = game over → score screen.

## Progression / addiction
- High score + best-body-reached persisted. Evolution chart (silhouettes of undiscovered tiers).
- Score combos; "New discovery!" banner first time each tier is made.
- Coins earned per merge → cosmetic jar themes in shop (2-3 cheap ones) to give coins a sink.

## Monetization (per shared spec)
- Rewarded: "Continue?" on game over — watch ad to remove top 30% of pieces once per run. Also "2x score for 60s" button.
- Interstitial after every 2nd game over.
- IAP: gems buy jar themes + a "bomb" consumable (clears 5 smallest bodies).

## Tuning
- Jar ~ 9:14 aspect, 9 tiers, radii ~ 14,20,27,36,47,60,76,95,118 (scale to canvas). Spawn tiers limited to first 5.
- Physics stable: fixed timestep substeps, cap velocity.
