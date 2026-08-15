# DD-03 v2 — Hook'd (fishing) — BETA scope

Folder: `games/03-hookd/index.html` (upgrade the existing file — keep rendering/economy core). Follow `docs/DD-00-shared-spec.md`. Goal alignment: convert the janky continuous-steer loop into a crisp two-skill loop (timing + steering), deepen collection/meta, keep every rewarded slot.

## v2 Core loop (REDESIGNED CONTROLS — this is the #1 change)
Phase 1 — **CAST (tap timing)**: tap once → a vertical **depth gauge** appears (right edge): an arrow oscillates down-up along it (~1.2s period). Tap again → locks cast depth where the arrow is. Gauge shows: reachable range (grows with Depth upgrade), and a green **sweet spot** band (top 15% of current max depth region) → locking inside it = "PERFECT CAST": +25% value this cast + sparkle. Deeper = richer fish, so timing skill = money.
Phase 2 — **DROP (cinematic, no input)**: hook plunges fast to locked depth with bubble trail; fish part around it. ~1s. No descent-avoidance mechanic anymore.
Phase 3 — **REEL (L/R steering)**: hook rises at reel speed. Steering is **discrete-feel**: hold LEFT or RIGHT half of the screen → hook accelerates that way (heavy, smooth, satisfying; release = drift straight). Desktop: ←/→ arrows or A/D or mouse-hold sides. Big translucent ◀ ▶ zone hints on first 3 casts. Steer INTO fish to catch, up to capacity.
Phase 4 — **HAUL**: surface → recap panel: itemized fish counting animation, combo bonus line, perfect-cast bonus line, total. Then upgrades.

## v2 Gameplay additions
- **Catch combo**: each catch within 1.2s of the previous → chain ×1.1 each (max ×2). Chain meter on hook. Sound pitch ladder.
- **PERFECT HAUL**: filling capacity to 100% in one cast → ×1.5 total + banner. (Pairs with capacity upgrades: buying capacity makes perfect hauls harder but richer — tension.)
- **Rare spawns**: 🧰 treasure chest (static, mid-depths: catch = gems), 🥾 boot (junk, worth $1, funny toast), 🐉 mythic leviathan (deepest band only, huge value, screen-wide celebration + slow-mo). Golden fish keep pity timer: guaranteed ≤ every 5 casts.
- **Bait system**: pre-cast toggle if owned: bait charge = +40% fish density this cast. Charges from missions/daily/rewarded ad.

## v2 Meta progression
- Upgrades (×1.18 curve): Depth · Capacity · Value · **Reel Speed** (new) · **Steadier Hands** (new: slows gauge arrow 4 levels max) · Fish Trap (idle $, keep).
- **Collection book**: 20+ species with silhouettes; first catch = gem reward + card flip animation. Milestones: 5/10/15/20 species → permanent +10% value each.
- **Missions**: 3 active, rotating pool (~10 templates: "3 perfect casts", "full haul", "catch a chest", "reach 800m"...). Gems/bait rewards.
- **Depth records**: deepest reached + biggest single haul shown on HUD stats popup.

## Monetization (keep all v1 + add)
- Rewarded: 3x sale (every 3rd haul) · "▶ Golden cast" (guaranteed sweet-spot lock + 2x value, 1/session-ish) · Double offline trap $ · Free bait charge.
- Interstitial: every 5th cast (skippable; Remove Ads kills).
- IAP sim: gem packs, Remove Ads, Golden Hook (+1 cap), Value Booster.
- 💰 dashboard: keep, per-placement counts.

## Quality bar for "beta"
- The gauge tap-timing must feel fair: arrow eases at ends (sinusoidal), input latency imperceptible, generous hit window on sweet spot at level 0.
- Reel steering: acceleration ~600px/s², max speed ~450px/s, slight hook tilt while steering; catching feels magnetic (small assist radius).
- First 3 casts teach: one hint line per phase, then never again.
- Save key bump to `hookd-save-v2`.
- Balance: first 3 min ≈ 8-10 buys; perfect-cast rate for an attentive player ~40%; mythic first sighting ~15-20 min.
