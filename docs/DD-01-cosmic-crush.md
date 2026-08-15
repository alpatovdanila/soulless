# DD-01 v2 — Cosmic Crush (Suika-like physics merge) — BETA scope

Folder: `games/01-cosmic-crush/index.html` (upgrade the existing file — keep its physics core, it's good). Follow `docs/DD-00-shared-spec.md`. Goal alignment: maximize session addiction (combo/fever), retention (meta unlocks, missions), and rewarded-ad surface, while staying clip-able.

## v2 Controls (MOBILE-FIRST — this is the #1 change)
- **Touch**: press and drag ANYWHERE on the jar area → the pending piece follows finger X (piece hovers above the jar, never under the finger). Release → drop. While dragging, show a dashed vertical guide + landing preview shadow.
- **Desktop**: mouse move aims, click drops (unchanged).
- Drop cooldown ~0.35s. Drags starting on UI buttons don't aim.
- Guide line + ghost outline of where the piece will rest (approx: straight down to first collision).

## v2 Gameplay additions
- **Chain combos** (exists, amplify): merges within 1.5s chain ×1, ×2, ×3… multiplier on score AND coins. On ≥×3: "COMBO ×N" banner, rising SFX pitch ladder, bigger shake.
- **Fever meter**: every merge fills a meter (decays slowly). Full → FEVER MODE 10s: all scores ×2, background shifts hue, particle density up, tempo-up jingle. Meter UI is prominent. Rewarded ad: "▶ Extend fever +8s" appears once per fever.
- **Milestone slow-mo**: first-ever creation of Star or SUN → 0.5s slow-mo + radial burst + banner. (Clip moment.)
- **Danger feedback**: within 80px of line → red vignette pulse + heartbeat SFX, intensity scales.
- **Power-ups** (consumables, bottom bar, usable mid-run): 💣 Bomb (clear 5 smallest — exists, surface it better), 🔄 Swap (reroll current+next piece), 🫙 Shake (physics nudge settles pile). Earn via gems or "▶ watch ad for 1 free power-up" (1/run).

## v2 Meta progression (retention)
- **Missions**: 3 active (e.g. "Make 2 Moons", "Score 5,000 in one run", "Chain a ×4 combo"). Reward: gems/coins. Refresh a slot on completion (pool of ~10 templates, difficulty scales with stats).
- **Unlock shop** (coins): jar themes (3+), piece skin packs (planets/fruit/emoji — same physics, cosmetic), each with preview. Gems: exclusive skin + permanent "+1 starting luck" (first 10 drops skew smaller).
- **Trophies row**: best score, best body, total merges, max combo — visible on game-over screen.
- **Stats-driven bragging**: game-over screen = shareable-looking scorecard (big score, tier icons, combo max).

## Monetization (keep all v1 + add)
- Rewarded: Continue (clear top 30%, once/run) · 2x score 60s · Extend fever · Free power-up.
- Interstitial: every 2nd game over (skippable 3s; suppressed by Remove Ads).
- IAP sim: gem packs, Remove Ads, gem skin, starting luck.
- 💰 dashboard: keep, include per-placement impression counts.

## Quality bar for "beta"
- No physics regressions: pile stays stable, merges reliable, 60fps with 80+ bodies.
- Every new system has visible UI affordance and a first-time one-line tooltip.
- Save migration: bump save key to `cc01_save_v2`; ignore old save.
- Balance: fever reachable ~1/2min of decent play; mission #1 completable in first session; SUN still aspirational.
