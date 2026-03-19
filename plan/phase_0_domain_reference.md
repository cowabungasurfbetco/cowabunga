# Phase 0: Domain & Viability Assessment — Canonical Domain Reference

**Project:** Surf Betting Prediction System
**Date:** March 16, 2026
**Status:** Phase 0 Complete — Awaiting Human Gate Review

---

## 0.1 — Competitive Structure

### Competition Format History & Structural Breaks

The WSL Championship Tour format has undergone several major structural changes since 2010. Each represents a potential data discontinuity that must be accounted for in modeling.

**Heat composition evolution:**

The most significant format change was the shift from 3-person to 2-person heats, which began in 2019. Prior to 2019, early rounds used primarily 3-person heats (and occasionally 4-person heats in Round 1). Starting in 2019, 2-person heats were introduced from Round 3 onward. This is a major structural break in the data: 3-person heats and 2-person heats are fundamentally different competitions. In a 3-person heat, a surfer can advance by finishing 2nd; in a 2-person heat, only the winner advances. This changes strategy, risk-taking, wave selection, and the meaning of "winning." The existing dataset (2010-2024) spans both eras, and any model trained across this boundary without accounting for it is learning from two different sports simultaneously.

**Current bracket structure (pre-2026):** Opening Round (twelve 3-person heats, 36 athletes; 1st and 2nd advance, 3rd to Elimination Round), Elimination Round (four 3-person heats; 1st and 2nd advance, 3rd finishes 33rd), Round of 32 (sixteen 2-person heats), Round of 16 (eight 2-person heats), Quarterfinals, Semifinals, Final (all 2-person single elimination).

**2026 format overhaul:** For the first nine events of 2026, the opening round features 4 x 2-person sudden-death heats composed of bottom five seeds plus wildcards. This is yet another structural break.

**Mid-Season Cut:** Introduced in 2022. After the first five events (typically at Margaret River), the field is reduced: men's from 36 to 22 (cutting 14), women's from 18 to 10 (cutting 8). Post-cut event fields include 2 wildcards per gender. This creates a significant strategic variable — surfers near the cut line face different incentive structures than those safely above or hopelessly below it. **In 2026, the mid-season cut is eliminated for the first time in five years.** Instead, field sizes adjust for postseason events (24 men, 16 women for the final two events).

**Finals Day:** Introduced in 2021. The top 5 men and top 5 women compete in a single-day, bracketed event at a predetermined location (Lower Trestles, 2021-2025). The bracket structure: Match 1 (seed 5 vs 4), Match 2 (winner vs seed 3), Match 3 (winner vs seed 2), Title Match (winner vs seed 1). The No. 1 seed has a structural advantage: they only need to win one heat, while the challenger must win two (best-of-three format in the Title Match). **Finals Day is discontinued after 2025.** Starting 2026, the world champion is determined by cumulative season points — a reversion to the traditional format.

**Decision Log Entry:** The dataset spans three distinct competitive eras: (1) 2010-2018 (predominantly 3-person heats), (2) 2019-2021 (transition to 2-person heats, introduction of Finals Day), (3) 2022-2025 (mid-season cut era + Finals Day). The 2026 season introduces a fourth era. For modeling purposes, the safest approach is to use 2019+ data as the primary training set (2-person heats only) and treat 2010-2018 data as supplementary with appropriate era flags. The exact boundary will be refined in Phase 5.

### Scoring System

Waves are scored on a 0-10 scale by a panel of five judges. The highest and lowest scores are dropped, and the surfer receives the average of the remaining three middle scores. A surfer's heat total is the sum of their two best-scoring waves (maximum 20 points). There is no limit on how many waves a surfer can catch and have scored — only the top two count.

The scoring scale has informal tiers: 0.0-1.9 (Poor), 2.0-4.9 (Fair), 5.0-6.4 (Good), 6.5-7.9 (Very Good), 8.0-10.0 (Excellent). Importantly, the effective scoring range adjusts daily based on actual wave conditions — on a poor day at a beach break, a 7.5 might be the practical ceiling, while excellent conditions at a reef break allow full 10-point scores. This means raw scores are not directly comparable across events or even across days within the same event without condition normalization.

**Judging criteria (five major elements):** (1) Commitment and degree of difficulty, (2) Innovative and progressive maneuvers, (3) Combination of major maneuvers, (4) Variety of maneuvers and speed, (5) Power and flow. Research indicates that aerial maneuvers are scored significantly higher than tube rides and turns, but aerial completion rates are lowest (approximately 45.4%), creating an explicit risk-reward tradeoff that varies by surfer style.

**Interference and penalties:** An interference occurs when a surfer hinders another surfer's scoring potential while that surfer has priority. First interference in a priority situation: the offending surfer's heat score is calculated using only their single best wave (loss of second-best wave). First interference in a non-priority situation: the offending surfer's second-best wave is halved. Second interference in the same heat: disqualification. If deemed intentional or unsportsmanlike, the surfer also loses their best event result in the season ranking calculation. Interference is not rare — it occurs in perhaps 5-10% of heats and can be decisive.

**Priority system:** No surfer has priority at heat start. Priority is gained by being next in the rotation and is lost when a surfer catches a wave or paddles for but misses one. The surfer with priority has unconditional right-of-way for any wave they choose. In the 2026 format, the higher-seeded surfer starts with priority, creating a structural first-mover advantage that correlates with skill level (a confounder for modeling).

### Men's vs. Women's Circuits — Side-by-Side Comparison

| Dimension | Men's CT | Women's CT |
|-----------|----------|------------|
| Field size (pre-cut) | 36 athletes | 18 athletes |
| Field size (post-cut, 2022-2025) | 22 athletes | 10 athletes |
| Events per season | Same as women's (11 in 2026) | Same as men's |
| Format differences | None | None |
| Prize money | Equal since 2019 | Equal since 2019 |
| Challengers Series field | 96 men | 64 women |
| CS qualification slots to CT | Top 10 men qualify | Top 6 women qualify |
| Betting market availability | Available on most platforms | Available on most platforms |

**Competitive parity:** The existing model results (MODEL_RESULTS_V2.md) show that the men's CT is more predictable than the women's CT across all model types. The Elo baseline achieves 80.45% accuracy on men's test data vs. 74.41% on women's — a 6 percentage point gap suggesting greater parity (less hierarchy) on the women's tour. This aligns with the smaller field size: fewer athletes means less stratification of talent levels.

**Decision Log Entry:** Both circuits should be modeled, but likely with separate models or at minimum separate parameters. The difference in predictability is substantial enough that a single model would underperform on one circuit. Separate models also allow circuit-specific features (e.g., the women's field may have different condition-performance interactions).

### Rankings & Points System

The WSL ranking system awards points based on finishing position at each CT event. First place earns 10,000 points, second place 7,800, declining thereafter. Season standings are the cumulative total of all points earned. Rankings determine seedings into heats: higher-ranked surfers are seeded to avoid each other in early rounds. Jersey color indicates ranking: yellow jersey designates the highest-ranked surfer at regular season events.

Rankings influence seedings, which influence who faces whom, which influences outcomes. This creates a feedback loop: the ranking system is both an input to and output of the competitive process. For modeling, this means current ranking is a proxy for skill but also causally affects matchups.

**Distinction between WSL rankings and project Elo:** The WSL's official ranking is a cumulative points system, not an Elo system. Throughout this project, "Elo" refers to the project's own Elo rating system (built during Phase 6), which is fundamentally different — it adjusts for opponent strength, while WSL rankings do not. This distinction must be maintained precisely in all future phases.

### Tour Structure

The WSL operates a three-tier system. The Qualifying Series (QS) is open to any surfer with WSL membership, with events ranked QS1000 through QS6000 by prize pool. In 2026, seven regional QS series feed into the Challenger Series (CS). The CS is the elite second tier: 96 men and 64 women compete, with the top 10 men and top 6 women qualifying for the Championship Tour. The CT is the top tier, with wildcards (2 per gender) joining every event. Wildcard selection criteria include: previous CT injuries, consistent CT performers with off years, or WSL discretionary choices.

### Competition Scheduling

Events operate within a "waiting period" — typically a 7-12 day window during which competition can be called on or off depending on conditions. The competition director assesses surf conditions and forecasts daily, running events only when conditions meet the event's standards. This is critically important for modeling because it means surfers do NOT compete in random conditions — there is a deliberate selection bias toward better-quality waves. The conditions during actual competition days are not representative of average conditions at that location; they are the best conditions within the waiting period.

This has a direct implication: condition data matched to competition days represents a non-random sample. Any model that uses condition features must account for the fact that these are "selected" conditions, not average or random ones.

---

## 0.2 — Domain Hypotheses & Causal Framework

### Framework Selection

After evaluating four causal inference frameworks, the recommended approach is a **hybrid DAG + Rubin Causal Model (RCM) framework:**

**Pearl DAG** is used for domain hypothesis specification — drawing the causal structure, identifying confounders, mediators, and colliders, and determining which variables to adjust for (and which NOT to adjust for). The DAG framework forces precision about causal direction and makes confounding visible. Its completeness theorem (via do-calculus) provides formal guarantees about what can and cannot be identified from observational data.

**Rubin Causal Model** is used for practical estimation — implementing propensity score matching, stratification, or inverse probability weighting to estimate conditional causal effects. RCM integrates well with machine learning and handles effect heterogeneity (e.g., how the effect of wave size differs across surfer types).

**Why not pure DAG or pure RCM:** Pure DAG without estimation loses predictive capability. Pure RCM/ML without causal structure ignores confounding and risks learning spurious correlations. The hybrid approach gives both causal understanding (why the model works) and predictive accuracy (what to bet on).

**Alternatives considered and rejected:** Structural Equation Models (SEM) require strong parametric assumptions (e.g., linearity) that are unlikely to hold in a domain with complex interactions. Pure do-calculus is a mathematical tool within the DAG framework, not a standalone alternative.

### Causal Graph — Primary Hypotheses

The following DAG describes the hypothesized causal structure for heat outcomes. Arrows indicate causal direction (X → Y means "X causes Y"). Variables are organized by category.

```
SURFER SKILL & FORM
  Career Ability ──→ Elo Rating ──→ Heat Win Probability
  Recent Form ──────────────────→ Heat Win Probability
  Career Ability ──→ Recent Form (partially)

CONDITION-SURFER INTERACTION
  Wave Conditions ──→ Condition-Skill Match ──→ Heat Win Probability
  Surfer Style ─────→ Condition-Skill Match
  Break Type ───────→ Condition-Skill Match
  Stance (R/G) ────→ Condition-Skill Match (via frontside/backside)

VENUE & FAMILIARITY
  Venue Familiarity ──→ Wave Selection Quality ──→ Heat Score ──→ Heat Win
  Home Advantage ─────→ Wave Selection Quality
  Prior Heats at Venue → Venue Familiarity

SITUATIONAL / CONTEXTUAL
  Season Standing Pressure ──→ Risk-Taking Behavior ──→ Heat Score
  Round of Competition ──────→ Fatigue ──→ Heat Score
  Travel Load (cumulative) ──→ Fatigue
  Psychological Momentum ────→ Confidence ──→ Heat Score
  Time of Day ───────────────→ Physiological State ──→ Heat Score

MATCHUP-SPECIFIC
  Opponent Elo ──→ Heat Win Probability (directly, as the opponent)
  H2H History ───→ Psychological State ──→ Heat Score
  Style Matchup ──→ Relative Advantage (e.g., power vs. aerial)

STRUCTURAL / PRIORITY
  Seeding ──→ Priority Assignment ──→ Wave Access ──→ Heat Score
  Seeding ──→ Opponent Quality (via bracket) ──→ Heat Win Probability
```

**Key confounders identified:**

1. **Seeding ↔ Skill ↔ Priority:** Seeding is correlated with skill AND directly affects priority assignment. Priority affects wave access. This means priority's effect on outcomes is confounded by skill. To isolate the causal effect of priority, we would need to control for skill (Elo) or find natural variation in priority assignment.

2. **Venue familiarity ↔ Career ability:** Elite surfers have competed at venues more times simply because they've been on tour longer. Venue familiarity and general ability are correlated. A surfer's strong performance at a venue might be skill, not familiarity.

3. **Recent form ↔ Condition fitness:** A surfer who's been winning recently might be fitter (the causal driver) rather than having psychological momentum. Recent form conflates physical state, mental state, and possibly favorable matchup draws.

4. **Wave conditions ↔ Scheduling selection bias:** Conditions during competition are selected (not random), so observed condition-outcome relationships may not generalize to hypothetical conditions.

### Multicollinearity Pre-Flags

The following variable pairs are flagged as likely to be highly correlated. Statistical validation (VIF, correlation matrices) will occur in Phase 5, but the intuitive map is established here:

| Variable Pair | Why Collinear | Which is More Causally Direct |
|---------------|---------------|-------------------------------|
| Career win rate ↔ WSL ranking | Both capture "how good is this surfer" | Elo rating (adjusts for opponent strength) is preferred over both |
| Recent form (10-heat avg) ↔ Season points standing | Both reflect recent performance | Recent form is more proximal; standings accumulate over longer period |
| Swell height (buoy) ↔ Wave face size (shore) | Related by physics, but not identical | Wave face size at shore is more causally direct for surfer performance |
| Age ↔ Years on tour | Highly correlated (r > 0.8 expected) | Years on tour is more relevant (experience mechanism) but age captures physical decline |
| Elo rating ↔ Current ranking | Both measure current skill level | Elo is more principled (adjusts for opponent strength); ranking is point-based |
| Wind speed ↔ Wave quality | Onshore wind degrades wave quality | Wind is the cause, wave quality is the effect; include wind as the causal variable |
| Venue win rate ↔ Career win rate | Good surfers win everywhere more often | Venue-specific rate relative to career rate (the delta) is the informative feature |

### Hypothesis Categories

**Surfer-level hypotheses:**
- Career form (Elo) is the strongest single predictor of heat outcomes.
- Recent form (last 10 heats) captures short-term fitness, confidence, and momentum.
- Stance (regular vs. goofy) interacts with wave direction to create frontside/backside advantage — a surfer on their forehand scores higher at breaks that favor their stance.
- Surfer style (power vs. progressive/aerial) interacts with wave type — power surfers dominate at heavy reef breaks (Teahupoo, Pipeline), aerial surfers excel at more open-face breaks (Trestles, Snapper Rocks).
- Age and experience have opposing effects — physical decline vs. accumulated knowledge. The net effect likely varies by break type.

**Condition-level hypotheses:**
- Wave size affects scoring distributions and may differentially favor certain surfer types.
- Break type (beach, point, reef, slab) is a primary contextual variable that interacts with surfer style.
- Wind conditions degrade wave quality and increase unpredictability, potentially favoring the underdog.
- Tide state affects wave shape at specific breaks (especially reef and point breaks).

**Interaction hypotheses (the potential edge):**
- Surfer A's advantage over Surfer B changes depending on conditions (a power surfer dominates at Teahupoo but loses to an aerial surfer at Trestles).
- Break familiarity matters — surfers with more heats at a specific break perform disproportionately better there. Research confirms this as "functional perceptual attunement" — experienced locals read waves more effectively.
- Home-break advantage is substantial and well-documented in research. Jordy Smith at J-Bay, Steph Gilmore at Snapper Rocks, and Pipeline locals like John John Florence all show outsized performance at home venues.

**Situational hypotheses:**
- Season standings pressure affects risk-taking. Surfers near the mid-season cut (2022-2025 era) may surf more conservatively or more desperately depending on personality.
- Cumulative travel fatigue across a season degrades later-season performance. Research confirms this is real and directionally dependent (eastward travel is worse).
- Psychological momentum from recent wins carries forward, but the magnitude of the effect vs. confounding with fitness/form is empirically uncertain.
- Time-of-day effects: research shows afternoon performance advantages in explosive power sports due to circadian rhythms, but this is likely a small effect relative to other factors.

---

## 0.2b — How Surf Betting Odds Are Generated

### The ALT Sports Data Pipeline

The primary mechanism for surf odds generation was established in 2023 through an official, multi-year partnership between the WSL and ALT Sports Data. ALT Sports Data ingests the WSL's real-time scoring feed, processes it through proprietary simulation-based models, generates market probabilities, and distributes these to sportsbooks (DraftKings, FanDuel, BetMGM, Bet365, Caesars, etc.).

This is a centralized model: most sportsbooks offering surf betting are receiving their initial odds from the same source (ALT Sports Data), not building independent models. This has important implications for edge-finding:

**If ALT's model is sophisticated and incorporates the same variables we would use (conditions, matchup history, form), then the market may already be efficient for those factors.** Our edge would need to come from variables ALT doesn't incorporate, or from superior modeling of variables they do incorporate.

**If ALT's model is relatively simple (e.g., primarily rank-based with limited condition interaction), then a more granular model could find edge.** Given that ALT works across many niche sports (dirt racing, skateboarding, etc.) and is unlikely to have deep domain expertise in surfing specifically, there is reason to believe their model may be more generic than specialized.

**How much does public betting action move lines?** In niche sports, lines are relatively less affected by public betting pressure. The volume is too low for recreational bettors to significantly move prices. However, this cuts both ways: if a small number of sharp bettors enter the surf market, they could move prices more easily, and the sportsbooks would notice faster.

**What we don't know:** Whether ALT incorporates condition-specific data, surfer-condition interactions, or just uses ranking/Elo-style inputs. Whether there is a human oddsmaker adjusting outputs. Whether individual sportsbooks adjust ALT's base prices with their own models. These unknowns represent the information gap we're trying to exploit.

---

## 0.3 — Market Efficiency Assessment

### The Case for Inefficiency (Reasons to Believe Edge Exists)

**Niche market dynamics:** Surf betting is a very new, very thin market. The formal ALT Sports Data partnership only started in 2023. Major sportsbooks have taken a "cautious entry," starting with basic markets limited to CT events. This is the profile of a market that hasn't been optimized by years of sharp-money action. NFL point spreads are efficient because billions of dollars and thousands of sophisticated bettors have hammered them for decades. Surf betting has had approximately 2-3 years of structured market-making.

**Centralized odds generation:** If most sportsbooks source from ALT Sports Data, there is limited price discovery. In an efficient market, multiple independent models compete and prices converge to "true" probability. In surf, one model sets the price and sportsbooks adjust at the margins. Any systematic blind spot in ALT's model propagates across the entire market.

**Condition-surfer interaction effects are likely underweighted:** The most promising edge appears to be in condition-specific matchup analysis. A rank-based model gives the higher-ranked surfer a blanket advantage. But a 10-foot Pipeline barrel and a 3-foot Trestles point break are different sports — the relative advantage between two surfers can flip completely based on conditions. If ALT's model doesn't incorporate break-specific, condition-specific surfer performance, this is the primary edge candidate.

**Research-backed home advantage:** Academic research documents "extreme and unique" home field advantage in surfing — surfers with deep local knowledge of a break (tidal patterns, sandbar formations, current behavior) outperform their ranking-implied probability. If the market prices based on global ranking rather than venue-specific performance, venue-adjusted models could find systematic edge.

**Small field, high variance:** With only 2 surfers per heat (post-2019), there is inherent variance. Even the best surfer in the world loses ~20-25% of heats. In high-variance environments, market prices tend to be less precise because the true probability is harder to estimate.

### The Case Against (Reasons Edge Might Not Exist)

**You're not the first person to think of this.** The existence of NXTbets (a dedicated surf betting analytics platform), the ALT Sports Data partnership, and betting coverage in Australia (where surf culture is deep) all suggest that people with resources and domain knowledge are already in this market.

**Data scarcity limits model quality.** Even with 17,605 men's heat records and 7,743 women's, this is a small dataset by modeling standards. The number of heats at any specific venue in specific conditions is even smaller. Overfitting risk is high — a model might appear to find edge in backtest that vanishes out of sample.

**The preliminary model results (96.78% accuracy with gradient boosting) are almost certainly overfitted.** These numbers are too high to be real predictive accuracy on unseen data. A gradient boosting model achieving 98% accuracy on men's CT data suggests data leakage or train-test contamination, not genuine predictive power. This needs to be re-evaluated with rigorous temporal cross-validation in later phases.

**Vig is a real hurdle.** Standard vig ranges 2-5%. No surf-specific vig data exists, but given the niche nature, it could be at the higher end. A model needs to be not just better than the market — it needs to be better than the market PLUS the vig.

**Low liquidity limits bet sizing.** Even if edge exists, niche markets have betting limits. You can't scale to meaningful returns if the maximum bet is $100-$500.

### Viability Verdict

The balance of evidence suggests there is a **plausible but unproven case for market inefficiency** in surf betting. The strongest arguments for edge are: (1) the market is very young (est. 2023), (2) odds generation is centralized through a single provider that may not have deep surf-specific modeling, and (3) condition-surfer interaction effects are likely underweighted by a generic model. The strongest arguments against are: (1) data scarcity makes it hard to build a model with genuine out-of-sample edge, (2) the preliminary model results showing 97%+ accuracy are almost certainly inflated and should not be trusted, and (3) even if edge exists, vig and liquidity constraints limit practical returns.

This is a "proceed with skepticism" verdict. The project is worth continuing to Phase 1, but with hard expectations: (a) the preliminary model accuracy will drop dramatically under rigorous validation, (b) any real edge is likely to be small (2-5% above the market, if it exists at all), and (c) the first honest test of viability won't come until Phase 8 (out-of-sample model testing) at the earliest.

---

## 0.4 — Platform Landscape Assessment

### Sportsbooks Offering Surf Betting

**US-based:** DraftKings, FanDuel, BetMGM, Bet365, Caesars, BetUS, Stake.com, BetOnline. Availability is state-dependent — not all states with legal sports betting offer surf markets on all platforms.

**Australian:** TAB, Neds/Ladbrokes Australia, Sportsbet, PointsBet, Unibet Australia. Australia has the most developed surf betting market, consistent with the sport's cultural prominence there.

**UK/International:** No confirmed WSL coverage from major UK books (William Hill, Ladbrokes UK). Coverage appears to be primarily US and Australian.

### Available Bet Types

**Common (most platforms):** Heat winner, event/outright winner, head-to-head matchups, championship/season winner futures.

**Less common:** To reach final/semifinals, wave score over/under props, total score over/under. Props are state-dependent and platform-dependent.

**Live betting:** Some platforms (BetMGM mentioned specifically) offer in-play betting with odds updating in real-time during heats.

**Most promising bet types for edge:** Outright event winner and head-to-head matchups are likely the best targets. Heat-winner bets may be more efficient (simpler binary outcome, more attention) while outright winner bets involve more variables and longer time horizons — potentially more room for a condition-aware model to find value. The specific bet type strategy will be refined in Phase 9.

### Sportsbooks vs. Exchanges

**Sportsbooks (DraftKings, FanDuel, etc.):** Take the other side of your bet. They lose when you win. Direct incentive to limit winning bettors. This is the primary platform type available for surf.

**Exchanges:** Betfair, Smarkets. Peer-to-peer model — the exchange takes commission regardless of outcome, so structurally more tolerant of winners. **However, no confirmed surf markets exist on any exchange.** This is a significant finding: the exchange path, which would be the safer long-term strategy, is currently unavailable for surf.

### The "Getting Limited" Problem

This is a real and serious constraint for niche sports betting. Key findings:

- Niche sports bettors face higher limitation risk than mainstream sports bettors because thin liquidity makes sharp action stand out faster.
- Sportsbooks detect edge through closing line value (CLV) tracking — if you consistently beat the closing line, you're flagged.
- Limitation typically manifests as stake reduction first, then restricted market access, then account closure in extreme cases.
- Detection is faster in niche markets due to smaller betting volumes.
- Props and exotic markets are the most vulnerable to limitation.

**Practical implication:** Platform diversification is essential from day one. Spreading bets across multiple sportsbooks (DraftKings, FanDuel, BetMGM, Australian platforms if accessible) extends the runway before any single platform limits the account. Bet sizing discipline is critical — consistent medium-sized bets are less conspicuous than occasional large bets.

### Prediction Markets

**Kalshi:** Federally regulated (CFTC) prediction market. Covers real-world events including sports. No confirmed WSL markets exist, but the platform is structurally capable of hosting them. Binary contract format (Yes/No, $0.01-$0.99). Available in all US states due to federal regulation. Commission-based, no incentive to limit winners.

**Polymarket:** CFTC-regulated, currently at $5.35B weekly notional volume. No confirmed surf markets. Could potentially host surf markets.

**Manifold Markets:** Play-money prediction market (not real stakes). Most accessible for custom market creation. Could be used to test demand for surf markets without financial risk.

**None of these platforms currently offer surf betting markets.** This is relevant information for 0.4b.

---

## 0.4b — Market Creation Contingency Assessment

This sub-step is **activated** based on the 0.4 findings. While traditional sportsbooks do offer surf betting, the lack of exchange and prediction market options limits the long-term strategy (especially given limitation risk on sportsbooks). Evaluating market creation is warranted.

### Feasibility of Creating Surf Markets on Prediction Platforms

**Kalshi:** Market creation requires submitting a proposal to the platform, which must pass regulatory review (CFTC compliance). The process is not self-serve — Kalshi curates its markets. A surf market proposal would need to demonstrate: (a) clear, objective resolution criteria (WSL official results), (b) sufficient potential trading volume, and (c) regulatory compliance. The resolution criteria are straightforward (WSL publishes official results), but demonstrating sufficient volume for a niche sport is the challenge. Kalshi has expanded aggressively into sports, so they may be receptive.

**Polymarket:** Similar regulatory framework. Has been focused on political and financial markets. Less clear whether sports expansion is a priority.

**The demand question:** This is the fundamental problem. Creating a market is technically feasible; generating enough liquidity to make it functional is the real challenge. Surf has a dedicated but relatively small fanbase. The WSL's digital audience is growing, but whether that translates to active trading on prediction markets is unknown.

### Assessment

Creating surf markets on prediction platforms is a **speculative but potentially viable alternative path** — particularly on Kalshi, which has been expanding aggressively into new categories. The core prediction model and causal analysis don't change regardless of platform. The recommendation is: (a) pursue traditional sportsbook betting as the primary path (it's available now), (b) monitor Kalshi and Polymarket for sports market expansion, and (c) if the sportsbook path hits limitation constraints in Phase 12, revisit prediction market creation as a pivot. This does NOT warrant a dedicated assessment or pause at this point — it's a noted contingency.

---

## Phase 0 Gate — Written Answers

### 1. Do I understand the sport's structure well enough to know where structural breaks exist in the data — for both the men's and women's circuits?

**Yes.** The major structural breaks are identified and dated:
- 2019: Shift from 3-person to 2-person heats (the most significant break)
- 2021: Introduction of Finals Day format
- 2022: Introduction of mid-season cut
- 2026: Elimination of mid-season cut and Finals Day; new opening round format

Both men's and women's circuits share these break points. The women's circuit additionally joined the men at several venues starting 2021 (e.g., Teahupoo). The dataset (2010-2024) spans the pre/post-2019 boundary, which is the most consequential for modeling.

### 2. Is there a plausible reason to believe the surf betting market is inefficient enough to exploit?

**Plausible but unproven.** The strongest arguments: (a) the market is very young (~2023), (b) odds generation is centralized through a single provider (ALT Sports Data), (c) condition-surfer interaction effects are likely underweighted in a generic model. The strongest counterarguments: (a) data scarcity limits model quality, (b) vig imposes a real hurdle, (c) the preliminary model results (97%+ accuracy) are almost certainly overfitted and should not be taken as evidence of genuine predictive edge.

Proceeding on a hypothesis of inefficiency, not on demonstrated evidence.

### 3. What are the 2-3 most promising bet types / market segments to target?

1. **Outright event winner bets** — Most complex market (multiple rounds, condition variance), most likely to be mispriced by a generic model. Condition-aware predictions could find systematic edge if the market doesn't account for break-surfer interaction.

2. **Head-to-head matchup bets** — Direct prediction of who beats whom, which is exactly what the model is designed to do. May be more efficient than outright markets (simpler binary outcome) but still likely to underweight condition-specific factors.

3. **Live/in-play betting** — If real-time condition data can be processed faster than the market adjusts, there may be edge in live markets during events. This is a Phase 11+ consideration.

### 4. What platforms are available, and what constraints do they impose? If platforms are limited, has the market creation contingency (0.4b) been evaluated?

**Available platforms:** DraftKings, FanDuel, BetMGM, Bet365, Caesars (US); TAB, Neds, Sportsbet, PointsBet, Unibet (Australia). No exchange markets exist for surf. No prediction market currently offers surf.

**Constraints:** State-dependent availability in the US, limitation risk (higher in niche sports), no exchange alternative for winners to migrate to. The market creation contingency (0.4b) has been evaluated — Kalshi is the most plausible platform for surf market creation, but it remains a contingency rather than the primary path.

### 5. Do I understand how odds are generated for surf, and does that understanding point to specific types of inefficiency?

**Partially.** Odds are generated centrally by ALT Sports Data using proprietary simulation models fed by the WSL's real-time scoring data. The specific model architecture and inputs are unknown. The centralization itself is the most informative finding — if ALT's model has systematic blind spots (e.g., insufficient condition-surfer interaction modeling), those blind spots propagate across every sportsbook simultaneously. The most promising inefficiency type is condition-specific mispricing: situations where break type, swell characteristics, and surfer style create matchup dynamics that a generic model wouldn't capture.

### 6. Is the causal graph drafted, with confounders and multicollinearity pre-flagged?

**Yes.** The causal DAG is documented in section 0.2 with explicit hypothesized causal directions, four major confounders identified (seeding-skill-priority, venue familiarity-ability, recent form-fitness, condition-scheduling selection), and seven multicollinearity pairs pre-flagged with assessments of which variable in each pair is more causally direct.

### 7. Am I still motivated to continue given what I've learned?

**This is a question only you (Levi) can answer.** What I can offer is an honest assessment: the case for edge is real but fragile. The market is young enough that inefficiency is plausible. But the data is scarce enough that proving edge will be difficult, and the preliminary model results (97%+ accuracy) should be treated as artifacts of overfitting, not as evidence of genuine predictive power. If you proceed, proceed with the expectation that the first several phases are about testing the hypothesis of edge, not confirming it.

---

## Decision Log

| # | Decision | Reasoning | Date |
|---|----------|-----------|------|
| D-001 | Model men's and women's circuits separately (or with separate parameters) | Women's CT is measurably less predictable (74% vs 80% Elo baseline); different field sizes and competitive dynamics warrant separate treatment | 2026-03-16 |
| D-002 | Use 2019+ data as primary training set; treat 2010-2018 as supplementary with era flags | 3-person to 2-person heat transition in 2019 is the most significant structural break; mixing eras without flagging trains the model on two different sports | 2026-03-16 |
| D-003 | Adopt hybrid DAG + RCM causal framework | DAGs provide structural clarity and confounder identification; RCM provides practical estimation tools that integrate with ML | 2026-03-16 |
| D-004 | Target outright event winner and H2H matchup bets as primary market segments | Condition-surfer interaction effects are most likely to be underweighted in these markets; live betting is a secondary consideration for later phases | 2026-03-16 |
| D-005 | Use traditional sportsbooks as primary platform; hold prediction market creation as contingency | Sportsbook markets exist now; prediction market creation is speculative and depends on demand | 2026-03-16 |
| D-006 | Treat preliminary model results (97%+ accuracy) as unreliable pending rigorous re-validation | Numbers are too high for genuine out-of-sample prediction; likely data leakage or insufficient temporal separation | 2026-03-16 |

## Assumptions Register

| # | Assumption | Status | Risk if Wrong |
|---|-----------|--------|---------------|
| A-001 | ALT Sports Data's model does not incorporate break-specific, condition-specific surfer performance | Unverified | If ALT already models this, the primary edge hypothesis collapses |
| A-002 | Historical competition data (2010-2024) is sufficient for training | Partially verified (17.6K men's, 7.7K women's heats exist) | If not enough granularity (e.g., insufficient condition data per heat), model capacity is limited |
| A-003 | The WSL scoring system hasn't undergone undocumented changes that would create hidden breaks in the data | Unverified | Stealth changes to judging emphasis would silently corrupt historical comparisons |
| A-004 | NXTbets odds data (~283 rows) is accurate and representative | Unverified | If NXTbets data is incorrect or unrepresentative, early calibration signals are misleading |
| A-005 | Surf betting markets will continue to grow (more platforms, bet types, liquidity) | Assumed based on trend | If the market contracts or sportsbooks exit surf, the project loses its monetization path |
| A-006 | The getting-limited problem can be managed through platform diversification | Assumed | If all platforms limit simultaneously or share limitation data, the betting strategy becomes impractical |

---

## Re-Anchor Check

Before moving to Phase 1, confirm:

- [x] This domain reference document is complete and covers all Phase 0 sub-sections (0.1 through 0.4b)
- [x] Structural breaks are identified and dated
- [x] Men's and women's circuits are compared side-by-side
- [x] Causal DAG is drafted with confounders and multicollinearity pre-flagged
- [x] Market efficiency assessment is honest (not optimistic)
- [x] Platform landscape is documented with constraints
- [x] Market creation contingency is evaluated
- [x] All 7 gate questions have written answers
- [x] Decision log captures key Phase 0 decisions
- [x] Assumptions register captures key Phase 0 assumptions

**This document is the canonical domain reference for the project. All subsequent phases should refer back to it. If any future phase's work contradicts something stated here, that is a drift event that must be flagged and resolved before continuing.**
