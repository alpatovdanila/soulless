# Finance forecast assumptions (shared across all 5 analyses)

Grounded in 2025-26 industry benchmarks (Gamesforum/InvestGame hypercasual report, GGA hybrid-casual data):

## Benchmarks used
- Rewarded video eCPM: $10-15 (US), ~$3-6 blended worldwide. We use **$8 blended**.
- Interstitial eCPM: ~$4-8 US, we use **$5 blended**.
- Pure hypercasual ARPDAU: $0.03-0.08. Hybrid-casual (ads+IAP): $0.15-0.50.
- D1/D7 retention benchmarks: hypercasual ~25%/7%, hybrid ~35%/18-22%.
- Merge games: highest rewarded engagement (~100 rewarded views per user lifetime); idle ~73.

## Scenario model (per game, first 6 months, zero ad spend)
Distribution is the bottleneck. Three scenarios:
- **Base (most likely)**: web launch (itch.io + Reddit + 2-3 web portals like CrazyGames/Poki). 5k-30k plays. Web-portal rev-share ads only. → $50-$500 total.
- **Good**: a portal features it (Poki/CrazyGames editorial) or one Reddit/TikTok post pops. 100k-500k plays. → $2k-$10k.
- **Hit (lottery)**: viral clip loop (only applies to clippable games). 1M+ plays, port to mobile justified. → $20k+.

## Per-game modifiers
- Clippability multiplies the odds of Good/Hit, not the Base.
- Session length × rewarded-slot frequency drives ads/DAU: idle games ~3-6 rewarded/day/user; skill games ~2-4.
- Portal acceptance matters more than stores: CrazyGames/Poki accept solo web games and pay rev-share (~$1-4 per 1000 gameplays effectively) — this is the realistic money path, not Google Play.

## Effort cost side
- Each prototype → shippable web game: ~1-3 weekends of polish (art pass, balance, SDK integration for portal).
- Mobile wrap (Capacitor/TWA + AdMob): +1 weekend, only after web traction.
