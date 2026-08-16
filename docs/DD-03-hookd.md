# DD-03 v5 — Hook'd — graphics overhaul + directional cast + hyper-casual UI + progression rework

## v5.7 — CLOSE camera: depth is traveled, not surveyed (supersedes v5.1 reach-framing)
User: 10 minutes in, still staring at the same water with the boat always visible — no feeling of going deep. The reach-based zoom is the cause: if everything fits on screen, no cast feels far.
- **Fixed close framing**: idle/aiming shows the boat + only the top ~25-35m of water (constant-ish; may creep slightly wider over many spool levels, e.g. +10% per 10 levels, but must always feel close). Max reach does NOT define the frame anymore.
- **The descent is the show**: during SINK and REEL the camera follows the hook closely; the boat scrolls off-screen; passing depth bands (palette darkening, decor, ruler labels flowing by) IS the sense of progress. Deep casts should feel like a journey down and back.
- **Aim generalizes**: direction = boat→finger (unchanged, screen space, mechanical); power fraction = finger's screen distance / dNorm (~45% of screen height) instead of world distance — full extension = full power = maxReach travel in that direction; gauge factor still applies. Early game (reach ≈ visible water) this behaves exactly like point-at-target for visible fish; later the same gesture drives deeper-than-screen casts. Arrow stays fixed-max-length relative-power (unchanged).
- **NO zoom-out on spool purchase** (user revision): no reveal flourish at all — deeper levels' contents stay a mystery every time; the only way to see deeper is to cast deeper. Camera NEVER leaves close framing (also: zoomed-out views break the boat pile and catch-load animations, which must stay readable at all times).
- Crate carrot: no longer needs to sit in idle view — it's discovered during casts near max depth (glint at the edge of vision); keep its "just beyond reach" placement.
- **Sink profile**: steady descent, short ease-out over the last ~15% of travel, minimum speed floor (no asymptotic crawl), ~0.25s beat at the deepest point (puff/sfx tick), then the reel's 0.5s ramp. Velocity continuous, travel budget exact.

## v5.6 — sell ceremony + lure economy (MAIN LOOP MUST BE MAXIMALLY SATISFYING)
- **Basic lure is consumable too** — no infinite/free lure. But it's ~always refunded naturally: selling a haul refunds the lures spent on it; an EMPTY cast refunds its lure immediately with a clear telegraph ("lure refunded" toast/flyback animation). Net: playing normally never depletes basic lures; shop sells refills; running dry → rewarded/shop paths (play never hard-blocks: final fallback = free cast with sponsored chip).
- **Sell ceremony (3-4s, never rushed, eased)**: pressing SELL spawns a burst of small icons representing everything received — coins for money, lure icons for refunds/bonuses, part/artifact icons. They pop out, hover briefly, then fly with easing (staggered) to their respective UI targets: coins → the money counter, each lure → its OWN slot in the selector, parts → upgrades button. Every receiving button squash-bounces per icon received, counters tick up in sync, cha-ching cascade throughout. Easing on every stage (pop-out overshoot, hover, fly-in curves).
- **No auto-open of shop/upgrades after selling** (overrides the earlier conditional auto-open — never auto-open; the ⬆ button is the only path).
- **Lure selector = radio buttons**: all lure types always visible as adjacent slots (icon + count), one active — not a flyout/dropdown. Acquired lures fly to their slot and the slot animates on receipt.
- **Money counter**: prominent, animates counting as coins arrive.
- Principle: every stage of the loop (cast, catch, surface, sell) gets animation + sound + effects attention.

## v5.4 AIM = POINT-AT-TARGET (final contract; supersedes ALL previous aim models)
The user's exact flow: "I move my finger where the fish are, release at the good moment, and the lure goes where my finger was (limited by power and range)."
- **Aim is the finger's ABSOLUTE position** — not a drag delta, not a direction vector. While aiming, the arrow runs from the boat toward the CURRENT finger point, purely mechanically, every frame, in screen space. No cone mirroring, no kinematics, no smoothing that can lag or flip. If the finger moves left, the arrow points left. Any direction into the water is valid (boat is center-screen; left and right both fully reachable).
- **Arrow shows RELATIVE power, fixed max length** (v5.5 revision): the arrow's maximum on-screen length is a constant (e.g. ~140px) regardless of zoom or world reach. Its drawn length = power fraction (min(dist-to-finger, maxReach)/maxReach) × that constant. It is a direction + relative-power indicator in its own units — NEVER a spatial ruler to the landing point (no billiard aim helper; the arrow must not imply where the lure lands).
- **Release** = cast toward the finger point: target = finger position (world-converted at current zoom), realized travel = min(dist, maxReach) × gauge factor (gauge timing unchanged — that's the "good moment"). Lure travels the straight line boat→target underwater after the minimal cosmetic hop; the hop must not displace the line (enter the water ON the boat→target line).
- **Boat: CENTER of the screen.**
- **Animation serves the mechanics, never distorts them**: whatever bob/flex/zoom is happening, the drawn arrow and the traveled path must agree with the finger in screen space at all times. Animate around the mechanical truth.
- **Fish do NOT repel from the lure on the way down** — remove the fish-parting/dodge during SINK entirely (skittish flee stays a REEL-phase reaction only, and only to fast steering). Sink-catching must feel like placing the lure into fish.

## v5.5 gauge + content + ruler
- **Gauge: bar GROWS with power, zone stays constant** (replaces "zone shrinks"): the optimal zone keeps a fixed absolute size; more power = the whole TRACK widens around it. Arrow full-travel TIME stays constant regardless of track width → arrow moves faster in px on wider tracks = naturally harder. Nothing ever visually shrinks as power grows.
- **Smooth zone blending + telegraphing**: perfect zone blends gradually into a "good" zone, good into "standard" (gradient, not hard edges; factor falloff already smooth — make the visuals match). Good and perfect releases are telegraphed: distinct flash/label/sfx tiers ("PERFECT!" vs "Good!"), zone glow pulse near-perfect.
- **More to aim at**: raise overall fish density noticeably; ensure clusters/schools that read as aim-worthy targets from the boat.
- **Comic boat pile**: stashed fish are never replaced/capped visually — all of them pile up on deck into a silly growing tower (fish rendered 2-3× smaller on the boat, jitter/rotation, sways with the boat). The absurd pile is the reward signal for a fat stash. Perf: high cap (100+), pre-render the lower pile if needed.
- **Never surface empty**: shallow bands get substantially denser cheap fish (schoolers/drifters) — an unskilled band-1/2 cast should fill the weight cap with cheap catch. Filling the lure is the baseline experience; skill/depth decides value, not whether you catch. Testable: expected encounters along a straight band-1 traversal ≥ level-0 capacity.
- **The tantalizing crate**: place something interesting (crate/wreck box) at a depth JUST beyond starting reach — visible from the surface framing, very close, not immediately reachable. It's the "one more spool level" carrot. After it's looted, respawn a similar carrot deeper (always slightly beyond current reach, off-screen churn rules).
- **Depth ruler moves to the LEFT edge**: small semi-transparent labels (as before), every 10m ticks/50m labels — purpose includes letting the player reference depths in feedback ("the crate at 20m").
- **Fix the sink→steer speed jump**: velocity must be continuous across the sink→reel transition and when steering input begins — no acceleration spike the moment the player touches the joystick. Blend/lerp any phase-based speed changes.
- **Cap lateral steering speed**: horizontal speed is not unlimited — clamp it to a sane max (somewhat below the vertical reel speed feel), so steering is placement, not flying sideways.
- **Post-sell upgrades popup only when affordable**: auto-open after a sale only if ≥1 upgrade is buyable with the new balance; otherwise no popup (manual ⬆ button always works).
- **Upgrade effect labels**: each upgrade card shows one tiny line with the concrete computed effect of the NEXT level ("+10% depth", "+3kg per cast", "+12% reel speed", "slower arrow / wider good zone", "+$N/h"). Refines the no-prose rule: one short effect line allowed, nothing more.
- **Pacing retune (v5.5)**: current progress is too slow. Targets: (a) FIRST spool levels come fast — level 1 within ~1-2 casts, first 3-4 levels inside the first few minutes; (b) depth-per-dollar RISES progressively — as levels grow, each dollar buys more meters (cost curve grows slower than depth×fish-value growth), so the game accelerates and opens up instead of hitting a grind wall; (c) keep the earlier "+10% depth per 5-10 min" as the mid-game steady state, but early game must be faster than that.

## v5 AMENDMENTS — upgrades & lures (override v4/v3 where they conflict)

### Currency: NO GEMS. Lures instead.
- Remove gems everywhere. The consumable is the **lure**: one selected lure is spent when a cast ends. Money is the only earned currency; lures are bought with money (shop), earned from missions/daily, or granted by rewarded ads.
- **Lure selector**: ONE place, fast to switch (big chip near the cast area or a one-tap flyout showing owned counts). Selected lure visibly attached to the hook.
  - **Simple lure** — default, effectively unlimited (auto-refills free) so play is never blocked.
  - **Smelly lure** — fish are attracted: magnet/attract radius on fish is much larger this cast.
  - **Search magnet lure** — catches NO fish; ×5 attract power for chests/artifacts/parts this cast.
- IAP sim: lure bundles + money packs (replace gem packs). v4's gem-cost-cast + break-even rule is VOID (superseded).

### Monetization v5 (replaces earlier ad-placement rules)
- **NO interstitials inside the casting loop.** Remove the every-N-casts interstitial and any mid-loop ad overlay. Ads are opt-in rewarded only.
- **"Negotiate" button on the sell flow**: watch a rewarded ad → +50% MONEY on this sale (lures returned are NOT boosted) + a chance to receive a bonus lure (rarity-weighted). This is the flagship placement.
- Other rewarded slots allowed outside the loop: +N lures when short, double offline trap income, golden cast (between casts only).
- **Shop = the IAP surface**: lure bundles, money packs, and COSMETICS — cool rods, net skins, boat skins (visible on the gear/boat, purely cosmetic). Simulated purchases as always; dashboard keeps per-placement counts.

### Upgrades rework (meaningful, not "depth number up")
Replace the current upgrade list with:
- **Bigger Spool** — more line ⇒ deeper casts. Leveled (×cost curve). PACING RULE: +10% depth ≈ each 5-10 minutes of active play early on (tune costs vs avg $/min so the player doesn't "overpay for fish").
- **Better Line** — max WEIGHT pulled per cast. Every catchable has a weight (fish by species/size, chests heavy). Hooking stops when weight cap reached (line strain meter near capacity; taut-line visual). Replaces fish-count Capacity.
- **Better Spinning** — TIERS (not levels): Rusty → Steel → Carbon → Pro → Legendary. Each tier upgrades the cast minigame: slower gauge arrow and/or wider optimal zone; Legendary = "always perfect throw". Replaces Steadier Hands. Tier params live in the same GAUGE config object.
- Keep: Reel Speed (small leveled), Fish Trap (idle income). Value multiplier: fold into nothing — remove (money balance comes from fish values directly).
- **Gear is visible**: lure color/shape per type (Simple/Smelly/Search Magnet each instantly recognizable on the hook), line color/thickness reflects Better Line level (rope tiers), rod skin/color reflects Spinning tier (Rusty brown → Steel grey → Carbon black → Pro gold trim → Legendary glow). Progress you can SEE on the equipment itself.
- Museum also logs lures used and parts; parts (⚙️, 5 = +1 free level) now apply to Spool/Line/Reel.

### v5.3 aiming fix (supersedes all previous aim/preview rules)
- **No trajectory dots.** Delete the dotted preview entirely. Stop simulating a "real fishing throw".
- **One simple arrow from the boat center**, pointing exactly where the throw will go (the underwater travel direction). Power makes the arrow LONGER, never repositions or bends it. What the arrow points at is what the cast does.
- **Power goes underwater, not into the air**: the air stage is a short fixed cosmetic hop (small arc near the boat, splash) — effectively identical at any power. ~All of the power budget = underwater travel distance along the aimed direction. Currently ~90% of power is wasted on air flight — that must be zero-ish.

### v5.2 playtest feedback
- **Aiming is DIRECT, not slingshot** (user found opposite-drag inverted/counter-intuitive): the cast arrow points FROM the boat TOWARD the finger's drag direction; arrow length ∝ finger delta (= power). Drag down-right = cast down-right. Keep the cast cone clamp and the gauge exactly as is.
- **Arrow must be much more visible**: thick, high-contrast (white core + dark outline), clear arrowhead; power % label readable in sunlight.
- **Decorative layers must not read as gameplay**: the giant translucent near-layer fish and the black silhouette lumps confuse ("what are those?"). Near-layer oversized fish: remove at band-1 framing (only fade in once zoom < ~0.4, at much lower alpha). Far silhouettes: recolor from black to deep desaturated blue-grey, keep them small/edge-placed at shallow framing; nothing decorative should look bigger or darker than catchable fish in the starting view.
- **Reel-up is ~2× slower**: same speed as the sink. The up-journey is the gameplay; give it time.
- **Gauge risk/reward INVERTED** (overrides the original "wider with drag" rule): more power = HARDER minigame — the optimal zone SHRINKS as drag/power grows (linear for now: ~30% zone at min power → ~10% at max). Short safe casts are easy; max-depth casts demand precision. Steadier Hands/Spinning bonuses still add width on top.
- **PERSISTENT FISH POPULATION (critical)**: fish must NOT respawn at cast commit — the fish the player aimed at must still be there when the hook arrives (modulo their own swimming/skittish reactions). World population lives continuously; churn happens only off-screen (fish drift in/out at edges over time). Bait/lure density effects = spawn-rate modifiers on the off-screen churn, never a visible swap.
- **Steering starts at splashdown**: the virtual joystick (and arrow keys) works from the moment the lure enters the water — during the SINK as well as the REEL. Sink steering lets the player place the lure before ascending (weaker lateral authority while sinking is fine); catching stays reel-only or becomes active whenever steering is... keep catching active during sink too if hook touches fish (simpler and more rewarding). One-time hint updates accordingly.
- **Darters/fast fish rework**: much slower overall, and NO ping-pong (burst forward then burst backward reads mechanical, not fishlike). Bursts should pick a NEW heading each time (small random turn from current heading, occasionally larger), with curved glide-and-coast between bursts — like real fish repositioning, mostly drifting one general direction and meandering. No fish should ever visibly reverse on the spot.
- **Flight stage minimal**: air time < 1s regardless of power — the lure shoots into the water fast (snappy whip, quick arc, splash). No slow ballistic float; the throw must read as one crisp action even at max power (and it must be visible in the current framing — no off-screen apex).
- **Band lines → depth ruler**: remove the unlabeled horizontal band-boundary lines (user: "what depth is this line?"). Replace with a minimalist right-edge depth ruler: small tick every 10m, tiny meter label every 50m (sparser when zoomed out), subtle alpha. Reinforces the real-scale metric world; no other in-water text.

### v5.1 playtest feedback (camera & motion)
- **Fish are too fast, too linear.** Global speed pass DOWN (especially shallow bands — shallow fish must read as easy targets); movement gets gentle non-linearity: sinusoidal wander, curved drift, slight speed oscillation. Darters keep bursts but rest longer.
- **Camera zoom = exploration.** At any moment the view should frame roughly the player's REACHABLE depth (plus a dark teaser sliver below), not the whole world. Start: zoomed into band 1 — big fish, visible progress within the band. Each Bigger Spool level effectively "unlocks" more visible world; undiscovered depth stays out of sight until you can cast into it. Implement as a world-scale/zoom factor tied to max depth (smooth-lerped on upgrade, with a satisfying zoom-out moment). Deep weird content must NOT be visible from the surface at start.
- **Boat sits LEFT of screen** (~15-20% x), casts arc rightward — more aim room, natural slingshot ergonomics. Aim cone adjusts accordingly (rightward only).
- **Real scale: 50px = 1m in world units.** Boat ≈ 5m (250 world px), fish sized in plausible meters (sardine ~0.2m, cruisers ~1-2m, leviathan ~10m+), depth labels must match actual world distance (100m = 5000 world px below the surface). Camera zoom (above) maps world→screen; the invariant is internal consistency — a "100m" cast must LOOK ~20 boat-lengths deep, not 4. Depth-meter readouts, bands, and reach all derive from the same px/m constant.

### Fish rework (v5): behaviors + depth distribution
- **Behavior archetypes** — species get one of several visibly distinct behaviors, not one shared swim:
  - *Schoolers*: small fish in loose groups, aligned drift (cheap boids: cohesion+alignment within the school only).
  - *Darters*: idle-then-burst movement, quick direction changes (trout-like).
  - *Cruisers*: big, slow, steady lanes; barely react.
  - *Skittish*: flee from the hook when it approaches fast (counterplay to reel steering; smelly lure overrides flight into attraction).
  - *Drifters*: jellyfish-style slow vertical bob, near-static (easy targets, low value).
  - *Lurkers*: deep, hug edges/bottom of a band, rare and valuable (leviathan is the extreme case).
- **Depth distribution pass**: density, value, weight, speed and rarity all keyed to depth: shallow = dense/cheap/light/slow (schoolers, drifters), mid = mixed (darters, cruisers), deep = sparse/expensive/heavy/fast-or-lurking. Value grows superlinearly with depth; weight correlates with value so Better Line gates deep hauls naturally. Rarity tiers (common/uncommon/rare/epic) per band with visible palette difference; museum unchanged.
- **Depth gets WEIRD** (approved, clip-bait): the deeper you go, the stranger it gets — visuals and species shift from normal fishing into eerie discovery. Shallow: normal fish, sunny water. Mid: odd colors creep in. Deep bands: bioluminescent species (glow spots/trails in near-darkness), anglerfish with actual light lure, ghostly translucent fish, sunken relics among the artifacts. Deepest: unsettling one-off silhouettes in the far layer (huge shadow passing, unblinking eye) that don't attack — pure atmosphere, thalassophobia-tier tension. Ambient audio darkens with depth (low drones). First encounter of each weird species = snackbar + museum entry like everything else. The reveal IS the progression reward: every spool upgrade should promise "what's down there?" — never show upcoming species in advance anywhere in UI.

### Long-term progression note (design intent, implement only the pacing)
Depth-number-alone gets boring; the long game will come from zones/gear/crew (see gitignored notes). For THIS build: implement the three upgrade lines + lures + pacing rule; keep curves steep enough that a 15-min/day player has visible progress for ~2-3 weeks (spool cost ×1.35+/level beyond mid-game is fine).

## v4 AMENDMENTS (override anything below that conflicts)

### Flow: no interruptions
- **Kill the haul recap overlay.** Caught fish land visibly "on the boat": small fish icons pile on the deck + a stash chip (count/value). The loop is cast → catch → cast again, uninterrupted.
- **SELL is a separate big button** (shows when stash non-empty): coins/gems burst out, counters count up with juice, stash empties. After the sell animation, the **Upgrades popup auto-opens** (also openable anytime via its own button). No other auto-popups ever.
- **New species / rare finds → achievement snackbar** sliding in at the top, auto-hides ~2.5s, and adds an unread dot to the **Museum** button. NEVER a modal mid-loop.

### Economy: gem-cost casts (keep it net-neutral for now)
- Each cast costs gems (e.g., 10💎). Selling a stash returns money PLUS gems ≈ what was spent on the casts that filled it (target: average player breaks even on gems, earns money; perfect casts/combos net slightly positive).
- Never hard-block play: if gems < cast cost → cast is free with a "sponsored cast" chip (counts as an interstitial impression) or offer "▶ +50💎" rewarded ad. Playing must always be possible.
- Extras in the water: 🧰 chests (money+gems), 🏺 artifacts (rare, museum-worthy, big gems), ⚙️ upgrade parts (5 parts of an upgrade = +1 free level, shown in Upgrades popup). All sold/banked with the stash, all tracked in Museum.

### Museum (replaces "collection book")
- Shows EVERYTHING ever caught with catch counts: all species, chests, artifacts, parts, boots. Unread-dot badge when new entries arrive. Records (deepest cast, biggest haul) live here too.

### UI restyle: hyper-casual, not CRUD
- **Load a cool rounded display font** (Google Fonts link, e.g. Baloo 2 / Fredoka; system-rounded fallback — graceful if offline). Big, chunky, outlined/shadowed numerals for money/gems.
- **BIG icons, minimal text**: icon-first buttons (💰 sell, ⬆ upgrades, 🏛 museum, 🎯, 🛒, 🔊), no captions where the icon is obvious. Remove: depth "band" labels, "PROTOTYPE — simulated monetization" footer, upgrade explanations and "≈N casts" hints, any instructional text beyond the 3 one-time tutorial hints.
- **Stylized chrome**: rounded fat-border cards/pills (2-3px, soft shadow + slight gradient), squash-bounce on every button press, pop-in transitions for popups/snackbars, animated counters (tween numbers), pulsing glow on affordable upgrades. Everything that changes should move.
- **Minimum on-screen controls** during play: top = currencies + tiny icon row; bottom = nothing during aiming/reeling except contextual (SELL when stash full enough, rewarded chips only between casts).
- Upgrade buttons inside the popup: big icon + level pips + price chip. No prose.

### Animation & sound polish (v4.1)
- Go heavier on eased, goofy animations everywhere (overshoot/bounce easings, squash & stretch).
- On surfacing, every caught fish **flies into the boat** one by one along cartoon arcs (staggered, spin + scale, plop into the deck pile with a tiny boat rock per landing).
- Selling → satisfying money sound (cha-ching cascade scaling with amount) synced to the counter tween and coin burst.
- Casting → meaty **whoosh** on release, then **splash** on water entry (layered: thump + droplets).
- Air vs water speed contrast must READ: lure is fast and whippy in flight, visibly decelerates the moment it enters water (splash masks the transition; underwater drag heavier).
- Every catch (fish, chest, artifact, part, boot) = satisfying haptic + audio feedback: short `navigator.vibrate` pulse (~15-25ms, slightly longer for rares; silently no-op where unsupported e.g. iOS) + a crisp catch pop with the existing combo pitch ladder; rare catches get a heavier buzz pattern + richer sound.

(Original v3 spec follows; sections A/B/C still apply except where amended above.)

Folder: `games/03-hookd/index.html` (upgrade in place; keep economy, meta, missions, collection, monetization, WebAudio from v2 — this revision replaces the CAST/DROP/REEL input model and the entire look). Follow `docs/DD-00-shared-spec.md`.

## A. Graphics: "2.5D" overhaul (canvas 2D, layered — NOT WebGPU; see note)
Goal: stop looking like flat shapes on a gradient. Build depth with layers and light:
- **Sky/surface**: animated sky gradient, sun with bloom, drifting clouds (2 parallax layers), waves as 2-3 overlapping sine bands with foam caps; boat bobs and tilts on the wave phase, rod flexes during cast.
- **Underwater depth stack** (back to front): far silhouette layer (dark, blurred large fish/rock shapes, slow parallax), mid layer (main gameplay fish), near layer (occasional large foreground fish/bubbles, faster parallax, slight transparency). Parallax offsets driven by camera.
- **Light**: god-ray shafts from the surface (2-3 rotated translucent wedges, subtle animation), caustic shimmer near surface, ambient light falloff with depth (existing band gradient, but smoother + vignette), fish get a subtle top-light gradient.
- **Fish rendering**: upgrade from ellipse+tail: 2-3 part bodies (body, tail, fins) with sinusoidal swim animation, per-species palettes/shapes, slight vertical bob; scale ∝ layer depth. Golden/mythic get glow.
- **Hook/line**: line rendered as a curve (quadratic through control point lagging behind hook) with tension; hook is a real hook shape with a small lure flash; splash particles + ring on water entry/exit.
- **Perf bar**: 60fps mobile. Pre-render static-ish layers to offscreen canvases where cheap. No WebGPU/WebGL — canvas 2D only (`ponytail:` note: WebGPU revisit only if fps proves insufficient).

## B. New cast model (REPLACES v2 two-tap gauge)
State machine: IDLE → AIMING (drag held) → FLIGHT (ballistic) → SINK → REEL → HAUL.

**AIMING**: player presses ANYWHERE and drags (any direction; expected mostly down/diagonal). From the boat's rod tip, draw a cast arrow opposite-ish to drag (slingshot feel): arrow direction = opposite of drag vector, clamped to sensible cast cone (10°–80° from horizontal, either side). Arrow length ∝ drag distance = **max power** (clamp at dMax ≈ 40% of screen height; show power % label). A faint dotted trajectory-preview arc matching current direction+power updates live.
- **Power gauge** appears top-center while aiming: horizontal track, arrow ping-pongs across it (sinusoidal ease at edges, period ~0.9s ÷ Steadier Hands bonus), **optimal zone centered**, zone width scales LINEARLY with drag distance: width% = 12% + 28%·(d/dMax). Gauge is parameterized in one config object `{period, zoneW, windBias}` (windBias skews arrow velocity asymmetrically — 0 for now) so future zones/upgrades plug in.
- **Release** = cast: realized power = maxPower × factor(gauge): inside optimal zone → 1.0 ("PERFECT" flash + sfx); outside → 0.55–0.95 proportional to distance from zone. Below minimal drag (~24px) = cancel, no cast.
**FLIGHT**: hook flies a ballistic arc (gravity), rod whip sfx, line trails; splash on entry.
**SINK**: hook sinks along momentum direction, decelerating; final depth+lateral distance ∝ realized power (max power+perfect ≈ current max depth upgrade). Camera follows. Fish drift aside near hook.
**REEL**: hook is winched back toward the boat (line shortens at reel speed; path curves up toward rod tip). **Virtual joystick steering**: tap-and-hold anywhere → that point becomes the joystick center (draw base circle + thumb); finger offset left/right of center = lateral steering force (clamped ~60px radius, deadzone 6px); release = drift. Steer into fish; catch combo/capacity/magnetic assist as v2. Desktop keeps ←/→/A/D.
**HAUL**: unchanged from v2 (recap, bonuses).

First-cast tutorial: 3 one-time hints ("drag down to aim", "release in the green zone", "tap & slide to steer").

## C. Systems alignment
- Upgrades: rename Depth → **Cast Power** (max realized depth/distance); Steadier Hands now ALSO widens optimal zone (+6%/lvl) besides slowing arrow; Reel Speed, Capacity, Value, Trap unchanged.
- Sweet-spot economy: PERFECT CAST keeps +25% value bonus (now = releasing in optimal zone).
- "Golden cast" rewarded = auto-perfect + ×2 (unchanged concept).
- Missions referencing "perfect cast" now mean gauge-perfect release. Depth records = deepest hook reached.
- Everything else from v2 (combos, rares, pity, bait, collection, missions, monetization, dashboard, saves `hookd-save-v2` keep-compatible or migrate to v3 key) stays.

## D. Quality bar
- Cast must feel ergonomic one-handed portrait mobile: drag from anywhere (no precise targets), generous cancel, no accidental casts from UI taps.
- The aim arrow + gauge + trajectory preview must be readable on 375px width without covering the boat.
- 60fps on mobile-class; degrade gracefully (drop far-layer blur first).
- No regressions in economy/meta/monetization; node --check + keep the headless test green (update pure-block tests for new gauge math: zone width linearity, power factor falloff, windBias hook).
