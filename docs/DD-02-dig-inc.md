# DD-02 — Dig Inc. (idle miner)

Folder: `games/02-dig-inc/index.html`. Follow `docs/DD-00-shared-spec.md`.

## Pitch
Run a mining empire. Tap to dig, hire managers to dig for you, go deeper for exponentially richer ore, prestige into "Gold Rush" for permanent multipliers. Numbers go up forever.

## Core loop
- Big central DIG button: tap = money (satisfying pickaxe hit: particles of ore chunks flying + thunk sound, button squash animation).
- Shafts list (unlock up to 8): each shaft has level (cost ×1.15/level, output up), and a Manager (one-time cost → shaft auto-produces without taps).
- Each new shaft is 10x cost, ~8x output. Depth meter shows total progress.
- Money formatted with suffixes; income/sec displayed.

## Progression / addiction
- First 3 minutes: unlock shaft 2 and 3, buy 2 managers, ~10 level-ups. Costs tuned so something is always affordable within ~20s.
- Milestones per shaft (25/50/100 levels → ×2 output) with celebration toast.
- Prestige at ≥ $1e6: reset for Gold Nuggets (+2% all income each, permanent). Prestige screen shows exactly what you'd gain.
- Offline earnings: on load, modal "While you were away: +$X" (capped 8h).

## Monetization (per shared spec)
- Rewarded: "▶ 2x income for 60s" (persistent button), "▶ Double" on the offline-earnings modal, "▶ Instant 1h of production".
- Interstitial after each prestige.
- IAP: gems → permanent +25% income booster, auto-manager pack, time warps.

## Tuning
- Shaft 1: base $1/tap, $1/sec with manager. Balance so prestige #1 lands ~6-8 min of active play.
