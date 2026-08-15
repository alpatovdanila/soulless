# DD-03 — Hook'd (Tiny Fishing-like)

Folder: `games/03-hookd/index.html`. Follow `docs/DD-00-shared-spec.md`.

## Pitch
Cast your line, drop deep, then reel up hooking as many fish as you can. Sell the catch, upgrade depth / hook capacity / value, reach absurd depths where the weird expensive fish live.

## Core loop (the Tiny Fishing loop, it's proven)
1. Tap to cast: hook drops; player steers it left/right (drag/mouse) to AVOID fish on the way down (touching a fish on descent stops the drop early).
2. At max depth (or on fish hit), reel phase auto-starts: hook rises, now steer INTO fish to catch them, up to hook capacity.
3. Surface: catch flies out, sells with cha-ching counting animation per fish. Money → upgrades.
- Side-view water, depth strata with color gradient darkening; fish sprites = simple canvas shapes (ellipse+tail), size/speed/value by depth band. Rare golden fish sparkle.

## Progression / addiction
- 3 upgrades, each ~×1.18 cost/level: Max Depth (opens new strata + new fish types), Hook Capacity (more fish per cast), Fish Value (sell multiplier).
- New depth band every few depth upgrades → "New species discovered!" toast + collection log (species seen).
- Each cast is a 15-30s dopamine arc: cast → tension → harvest → payout. Always show next upgrade affordable-in-N-casts.
- Offline: passive "fish trap" income once bought (small $/sec, capped).

## Monetization (per shared spec)
- Rewarded: "▶ 3x sale" button on the payout screen (appears every 3rd cast), "▶ Instant depth boost" (one cast at +50% depth).
- Interstitial every 5th cast (skippable, 3s).
- IAP: gems → permanent golden hook (+1 capacity), value booster; remove ads.

## Tuning
- Start: depth 120px-worth (band 1, minnows $1-3), capacity 3. First 3 min: ~8 upgrade buys, reach band 2-3.
