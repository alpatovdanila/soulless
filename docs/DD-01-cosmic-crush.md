# DD-01 v3 — Bouba & Kiki (soft-body merge) — BETA scope

Folder: `games/01-cosmic-crush/index.html` (folder name kept for link stability; the game is **Bouba & Kiki**).
Follow `docs/DD-00-shared-spec.md`. Goal alignment: maximize session addiction (combo/party), retention
(meta unlocks, missions), and rewarded-ad surface, while staying clip-able.

v3 replaces the Cosmic Crush space theme wholesale: new cast, new physics engine, new minimal UI.

## The cast — one alternating chain
Two kinds of friend live in the meadow together. **Boubas** are soft-body and always smiling.
**Kikis** are solid and a bit grumpy, but they're good guys.

| # | Friend | Shape | Kind | Notes |
|---|--------|-------|------|-------|
| 0 | Tri Kiki | triangle | solid | the smallest, near-rigid |
| 1 | Square Bouba | 4 corners | soft | |
| 2 | Penta Kiki | 5 corners | solid | |
| 3 | Hexa Bouba | 6 corners | soft | |
| 4 | Star Kiki | 7-pointed star | solid | |
| 5 | Big Bouba | round | soft | the goal; squishiest of all |

Two of the same make the next one up. Corner rounding: Kikis get a tiny radius (sharp but friendly),
Boubas get a large one (pillowy).

## Physics — real soft bodies, not an animation
Every friend **is** a ring of point masses; there is no rigid core. Position and rotation are read back
out of the points by shape matching (Müller et al. polar decomposition), so an off-centre shove genuinely
translates, spins and deforms the body. Solver is position-based (PBD): predict → constrain → derive
velocity, which is unconditionally stable.

- **Shape matching stiffness** is the softness knob. Kikis use 1.0, which makes the same solver produce an
  exactly rigid body. Boubas ramp 0.52 → 0.10 as they grow.
- **Membrane + pressure**: perimeter springs plus an internal gas constraint keep Boubas from collapsing
  while their shape flows. All three stiffness knobs soften together with size — softening only one lets
  the others cancel it out.
- **Contact**: point-vs-outline for every pair, with the reaction spread across the hit edge's two points,
  so bodies dent each other and torque emerges. Merges fire on real outline contact, not centroid distance
  (centroid distance misses corner-to-corner touches).
- **Rotation is never corrected upright** — friends spawn at a random angle and lie however they land.
- Frozen friends are held perfectly stiff until thawed.

Measured: 22 bodies / 478 points costs 0.75ms per frame (22× the 60fps budget) on desktop; ~4.5ms on a
phone assumed 6× slower. No library needed — see "Why no physics library" below.

## Tools — the only consumables
Bomb/Swap/Shake are gone. Two tools remain, as big stylized buttons filling the bottom bar:
- **🔻 Spike** — arms the next drop. Rides the normal aim/release flow (guide line + landing ghost) and
  falls as a heavy rounded triangle that shoves the crowd around organically. Tap it to smash it away.
- **🌀 Mixer** — 3.5s vortex that stirs everybody. The danger timer is suspended while stirring so the
  stir itself can't end the round.

**Earning them: frozen friends.** ~13% of drops arrive encased in ice with a tool visible inside. They
can't deform while frozen. Match one to thaw it and collect the tool. Also buyable with gems, and the
rewarded-ad "free tool" grants one per round.

## Controls
- Press and drag anywhere → the pending friend follows finger X, hovering above the meadow. Release to drop.
- Dashed vertical guide + dashed landing ghost, both drawn as the real shape at its real spawn angle.
- Drop cooldown ~0.35s. Drags starting on UI buttons don't aim.

## Look
Cheerful green scenery: sky gradient, drifting clouds, a sun, two layers of rolling hills, a grass meadow
penned by wooden posts, grass blades waving along the ground. Cartoon font throughout (Comic Sans stack).
Themes: Green Meadow (free), Sunset Hills, Blossom Grove, Firefly Night. Palettes replace the old glyph
skins: Classic, Pastel, Candy, Neon.

## UI — minimal, playfield-first
The canvas fills the whole screen; everything else floats over it.
- Top-left: big score. Top-right: three small pills (▶2× rewarded, shop, menu).
- Thin party meter strip under the score; one mission chip; both tap through to the menu.
- Bottom: the two big tool buttons with count badges.
- Everything else (wallet, friend chain, full mission list, daily gift, free-tool ad, mute, money
  dashboard) lives behind the ☰ menu.

## Gameplay systems (carried over)
- **Chain combos**: merges within 1.5s chain ×1, ×2, ×3… on score AND coins. ≥×3 shows a banner.
- **Party meter** (was Fever): fills on merges, decays slowly. Full → 10s of ×2 everything, rainbow glow.
  Rewarded ad extends it +8s, once per party.
- **Milestone slow-mo**: first-ever Star Kiki or Big Bouba → 0.5s slow-mo + shockwave rings + banner.
- **Danger feedback**: red vignette pulse + heartbeat SFX scaling with pile height.
- **Missions**: 3 active from an 11-template pool, including "Thaw N frozen friends". Rewards gems/coins,
  slots refresh and scale on completion.
- **Trophies**: best score, best friend, total matches, total thawed.

## Monetization (unchanged surface)
- Rewarded: Continue (clear top 30%, once/round) · 2× score 60s · Extend party · Free tool.
- Interstitial: every 2nd game over (skippable 3s; suppressed by Remove Ads).
- IAP sim: gem packs, Remove Ads, gem palette, starting luck.
- 💰 dashboard with per-placement impression counts.

## Why no physics library
No JS/WASM 2D library ships true deformable soft bodies — the rigid engines (Matter.js, Planck, Rapier,
Box2D-wasm) either fake it with circles-on-springs or don't offer it at all, and none of them would give
the size-graded squish this game is built on. The custom PBD/shape-matching solver is ~180 lines, has no
download cost, and leaves 20×+ frame headroom. Revisit only if the body count grows by an order of magnitude.

## Quality bar for "beta"
- Piles settle to exact rest (verified: zero residual velocity), no NaN, no jitter when a friend is
  squeezed between two others.
- Every new system has a visible affordance and a first-time one-line tooltip.
- Save key `bk01_save_v1`; old Cosmic Crush saves ignored.
- Balance: party reachable ~1/2min of decent play; mission #1 completable in first session; Big Bouba
  still aspirational.
