# Shared Spec — all 5 prototypes

Applies to every game. Each game = ONE self-contained `index.html` (vanilla JS, no build, no CDN, no external assets). Must run from `file://` and localhost.

## Hard requirements
- Mobile-first: touch + mouse, responsive, works at 375px wide and on desktop. `<meta name="viewport">`.
- Playable in first 5 seconds. No menus before gameplay. Tutorial = one floating hint line max.
- Save/load via localStorage (key prefixed with game id). Offline progress where genre-appropriate.
- Juice is mandatory: particles on rewards, number pop/tween on gains, screen shake on big events, WebAudio-generated SFX (no audio files) — short blips/pops, satisfying merge/coin sounds. Subtle. A mute button.
- 60fps target; requestAnimationFrame; keep it simple.

## Monetization emulation (REAL mechanics, FAKE transactions)
Every game implements, visibly and naturally in the loop:
1. **Rewarded ad**: buttons like "▶ 2x for 60s", "▶ Double offline earnings", "▶ Continue/Revive". Clicking shows a fake full-screen "ad" overlay with a 5s countdown ("Simulated Ad — 5s"), then grants reward. Track impressions.
2. **Premium currency + IAP shop**: gems earned rarely in play; shop with fake IAP packs ($1.99/$4.99/$9.99 — clicking shows "Simulated purchase" confirm and grants gems) and one "Remove Ads $2.99".
3. **Interstitial moment**: after every N sessions-worth of actions (e.g., every prestige / every 3rd game over), show a skippable fake interstitial (3s). Track impressions.
4. **💰 Monetization dashboard**: a small toggleable panel showing: session length, rewarded impressions, interstitial impressions, simulated revenue (rewarded eCPM $12, interstitial eCPM $6, i.e. $0.012 and $0.006 per impression), fake IAP revenue. This is the "communicate the monetization path" requirement — make it obvious and honest.

## Addiction/retention mechanics (per genre, pick what fits)
- Near-term goal always visible (next unlock/upgrade cost).
- Escalating numbers with suffixes (1.2K, 3.4M, 5.6B...).
- Prestige/soft-reset where genre-appropriate.
- Daily-bonus stub (button, resets on real date change).

## Quality bar
- No console errors. No dead buttons. Balance tuned so first 3 minutes deliver ~8-12 meaningful upgrades/unlocks (fast early curve).
- Game title + one-line pitch on screen (small header).
- Footer badge: "PROTOTYPE — simulated monetization".
