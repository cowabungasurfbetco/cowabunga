# Surf Betting Prediction System — Master Project Plan (v2)

---

## How to Use This Document

This plan is designed to be executed sequentially with hard gates between phases. Each phase ends with a **GO / NO-GO checkpoint** — a set of conditions that must be true before you move forward. Some phases have internal checkpoints as well.

Throughout the document, three types of checkpoints appear:

- 🔴 **HUMAN GATE** — You must manually review and approve before proceeding. These are non-negotiable stopping points where compounding errors are most likely if skipped.
- 🟡 **AGENTIC CHECK** — An automated validation runs and surfaces results for your review. You don't need to deeply audit every one, but you should scan the output and flag anything surprising.
- 🟢 **AUTOMATED GUARD** — Runs silently in the background and only surfaces an alert if something fails a threshold. Think of these as tripwires.

A fourth checkpoint type is introduced in this version:

- 🔵 **RE-ANCHOR CHECK** — A periodic instruction for agents (and yourself) to re-read this master plan document and the Phase 0 domain reference document, then verify that current work hasn't drifted from the core principles, phase structure, or decision log. If drift is detected, stop all work and flag it for human review. These are placed at every major phase transition and at random intervals within longer phases to catch gradual drift early.

A core principle: **every phase should produce an artifact you can inspect** — a summary table, a visualization, a distribution plot, a sample of records — not just code that ran without errors. "It ran successfully" is not the same as "it did the right thing."

---

## ⚠️ Fresh Start Directive

**All data, models, and analysis in the `old/` directory are considered untrusted and must not be used.** Previous end-to-end attempts contained likely data leakage (97%+ model accuracy is not credible for this domain) and unvalidated assumptions. This plan starts from zero. Any code, CSVs, model artifacts, or reports from prior work should be treated as historical reference only — never as inputs to new work. If a previous finding seems useful, it must be independently re-derived and validated from scratch.

---

## Phases at a Glance

| Phase | Name | Primary Output | Key Gate |
|-------|------|----------------|----------|
| 0 | Domain & Viability Assessment | Written domain model + viability verdict | 🔴 Is there a plausible edge? |
| 1 | Data Landscape Audit | Inventory of available data sources | 🔴 Is there enough data to proceed? |
| 2 | Architecture & Schema Design | ERD, ETL diagram, tech stack decisions | 🔴 Approve schema before any code |
| 3 | Data Collection — Small Scale Pilot | ~1 season of data, fully inspected | 🔴 Manual data review |
| 4 | Data Collection — Full Scale | Complete historical dataset | 🟡 Automated DQ + human spot-check |
| 5 | Feature Engineering & Selection | Candidate feature set with rationale | 🔴 Feature review + sanity check |
| 6 | Model Research & Selection | Documented comparison of approaches | 🔴 Model choice with written justification |
| 7 | Model Build & Training | Trained model(s) with diagnostics | 🟡 Overfitting checks + calibration review |
| 8 | Model Testing & Validation | Out-of-sample performance report | 🔴 Is the model good enough to bet with? |
| 9 | Betting Algorithm Research | Written comparison of strategies | 🔴 Strategy choice with justification |
| 10 | Betting Strategy Backtesting | Simulated P&L across multiple years | 🔴 Would you trust this with real money? |
| 11 | Paper Trading | Live predictions without money | 🔴 Does live performance match backtest? |
| 12 | Live Deployment — Micro Stakes | Real bets at minimum amounts | 🔴 Ongoing performance review |
| 13 | Ongoing Monitoring & Iteration | Dashboards, drift detection, retraining triggers | Continuous |

---

## What You Listed vs. What I've Added

Before diving into the phases, here's an honest accounting of what you already identified and what I'm recommending you add, along with why.

**Phases you identified that map directly to the plan:**
Data Collection, Data Quality Checks (cross-cutting), Model Build, Model Testing, and Betting Algorithm. These are all here, some split into sub-phases for additional gate opportunities.

**Phases I'm adding that you didn't mention:**

**Phase 0 — Domain & Viability Assessment.** This is arguably the most important addition. Before you build anything, you need to answer: "Is the betting market for surf inefficient enough that a model could find edge?" If the market is efficient (meaning the odds already reflect all available information), then even a perfect model won't make money. You also need to deeply understand the sport's structure, because format changes over time (3-person heats to 2-person heats, introduction of Finals Day, mid-season cuts) create structural breaks in the data that can silently corrupt your model if you don't account for them.

**Phase 1 — Data Landscape Audit.** Before designing an ETL pipeline, you need to know what data actually exists, how far back it goes, how complete it is, and what format it's in. This prevents the very common mistake of designing a schema around data you assume exists but doesn't. This is a research phase, not a building phase.

**Phase 11 — Paper Trading.** This is a critical buffer between "the backtest looks good" and "I'm betting real money." You run the system live, making predictions and recording what bets it would place, but without any money at risk. This catches a whole category of problems that backtesting misses: data latency issues, odds that move before you can place a bet, events that get cancelled or restructured mid-competition, and the psychological element of whether you'll actually follow the system when it tells you to bet on something you disagree with.

**Phase 12 — Micro Stakes.** Even after paper trading, the first real-money phase should use the minimum possible bet sizes. This is about catching platform-specific issues (bet rejection, odds discrepancies between what you see and what executes, withdrawal limits) with minimal financial exposure.

**Cross-cutting elements I'm adding:**

**Decision Log.** Every significant choice (which data source, which model, which features, which betting strategy) should be recorded with the reasoning at the time. When something goes wrong later, the decision log lets you trace back to where the chain broke instead of guessing. This also prevents re-litigating decisions you already made.

**Assumptions Register.** Every phase involves assumptions — "this data source is reliable," "the scoring system hasn't changed," "historical odds reflect what was actually available." These should be explicitly written down and periodically re-validated. Many compounding errors come from assumptions that were true when made but silently became false.

**Sensitivity Analysis.** At key decision points, you should test: "If this assumption is wrong by X%, how much does it change the outcome?" This tells you which assumptions matter most and deserve the most scrutiny.

**Versioning Strategy.** We will use Git for version control (GitHub username: `cowabungasurfbetco`). Before any code is written in Phase 2, we need to set up the repository, establish branching conventions, and confirm access. Everything — raw data snapshots, feature engineering code, trained model artifacts, betting algorithm parameters, and this plan itself — will be version-controlled. When performance changes and you don't know why, Git history is how you figure it out. A pre-execution step at the start of Phase 2 will cover repo setup and access verification.

**The "Edge Decay" Problem.** Even if you find a profitable edge, edges in betting markets tend to shrink over time as markets become more efficient, betting platforms adjust, and other bettors find similar patterns. Your monitoring system needs to detect when your edge is eroding, not just when your model is drifting.

**Platform Risk Management.** You raised this — the risk of getting limited or banned for winning too much. This isn't just a concern to note; it needs to be an active design constraint that influences your betting algorithm (bet sizing, frequency, platform diversification, whether exchange-based models are preferable to sportsbook models).

**Re-Anchor Checks.** A system of periodic re-reads of this master plan and the Phase 0 domain reference document, placed at major phase transitions and at random points within longer phases. The purpose is to catch drift — situations where the current work has gradually departed from the agreed-upon principles, structure, or decisions without anyone noticing. When an agent or you perform a re-anchor check, the specific action is: re-read this document, re-read the Phase 0 domain reference, compare current work to the plan, and flag any discrepancies. If discrepancies are found, stop and discuss before continuing.

---

<a name="phase-0"></a>
## Phase 0: Domain & Viability Assessment

**Purpose:** Understand the sport, the betting market, and whether this project is worth building before writing a single line of code.

**Why this phase exists and why it comes first:** The most expensive mistake isn't a bad model — it's building a good model for a market where no edge exists. This phase forces you to answer the fundamental question early: "Is there a reason to believe that publicly available information, processed well, could predict surf outcomes better than the betting market already does?"

### 0.1 — Understand the Competitive Structure

Research and document the following. The output should be a written reference document (a few pages) that serves as the **canonical domain reference** for the entire project. Both you and any agents working on subsequent phases should refer back to this document regularly. This document will be explicitly referenced in re-anchor checks throughout the plan — if the domain reference says the scoring system works a certain way, and a later phase is treating it differently, that's a drift event that must be flagged and resolved.

**Men's and Women's circuits as independent tracks:** The men's and women's WSL circuits may differ in meaningful ways — field depth, scoring distributions, competitive balance, format nuances, the degree of dominance by top surfers, and critically, betting market availability and depth. Research both circuits independently from the start. Produce a side-by-side comparison covering: number of athletes, competitive parity (how often does the top-ranked surfer actually win?), scoring distributions, format differences, and betting market availability for each. The comparison should inform a deliberate decision about whether to model both circuits, just one, or both but with separate models. This decision goes in the Decision Log.

**Competition format:** How many surfers per heat? How has this changed over time? (The WSL shifted from 3-person to 2-person heats — this is a major structural break in the data.) How does the bracket work? What are the round names and elimination structure? Has it changed? What is the mid-season cut? When was it introduced? How does the Finals Day format work, and when was it introduced? Also document: penalty systems and their implications (interference penalties, priority violations — how do these affect scoring and heat outcomes?), and the guidelines for competition scheduling (what conditions determine whether a competition day is called "on" or "off," what causes event delays or cancellations, and how does the waiting period system work?). These scheduling dynamics matter because they affect which conditions the surfers actually compete in, and understanding the decision process for calling a competition day helps predict what conditions to model.

**Scoring system — deep dive:** Wave scores operate on a 0-10 scale with the best two waves counting toward a surfer's heat total (maximum 20). But the scoring system deserves a deeper treatment than that summary. Research and clearly document: how judges score individual waves (what criteria — commitment, difficulty, innovation, variety, speed, power, flow), how the panel of judges works (how many judges, how scores are aggregated, whether outlier scores are dropped), and whether there have been documented changes to judging criteria or emphasis over the years. Also important: what happens in unusual situations like interference calls — how does a penalty affect the non-offending surfer's score? How does a second interference (which results in disqualification) work?

**Score normalization for conditions (added post-Phase 0 review):** Raw wave scores are not directly comparable across events or even across days within the same event. The effective scoring ceiling adjusts based on wave quality — a 7.5 might be an elite score on a poor day but mediocre on an excellent day. Phase 5 must include a condition-normalized scoring system: either (a) z-score normalization within each event/round/day, (b) a condition-adjusted score model that estimates what a "7.0" means relative to the day's maximum achievable score, or (c) percentile-based scoring relative to all scores in that heat or round. The specific approach should be evaluated in Phase 5, but the requirement is established here: no model should ingest raw scores without condition context.

**Judging criteria differential weighting hypothesis (added post-Phase 0 review):** Research indicates the five judging criteria (commitment, difficulty, innovation, variety, speed/power/flow) do NOT contribute equally to actual scores awarded. Aerial maneuvers score significantly higher (~7.40 avg vs. ~5.08 for traditional maneuvers) but have the lowest completion rate (~45.4%). This creates an explicit risk-reward tradeoff that varies by surfer style. Hypothesis: the relative importance of each judging criterion has shifted over time (toward progressive/aerial surfing) and may weight differently in men's vs. women's competition. This hypothesis should be tested during Phase 5 (Feature Engineering) by analyzing score distributions by maneuver type if wave-level data with maneuver classification is available. If not available from structured data, this becomes a primary candidate for the video analysis workstream (see Phase 5b).

**Men's vs. women's judging differences (Phase 0 finding):** Men and women use identical judging criteria and scoring scales. No published research documents direct judging bias by gender. However, structural differences affect scoring patterns: (a) aerial maneuvers are "extremely rare" in women's competition compared to men's, due partly to biomechanical differences and partly to historical training access disparities; (b) women's events historically received inferior wave conditions (fewer competition days, weaker swell windows); (c) the smaller women's field (18 vs. 36 pre-2026) means fewer matchup data points per surfer. These structural differences — not judging bias per se — are the primary drivers of the predictability gap between circuits (men's Elo baseline: ~80% accuracy; women's: ~74%). Separate models for each circuit should account for these patterns.

**Interference analysis data requirement (added post-Phase 0 review):** During Phase 3-4 (data collection), build an interference data table capturing: surfer ID, event, round, heat, whether the surfer committed the interference, whether they had priority, whether it was a first or second offense, the score penalty applied, and the heat outcome. This table enables Phase 5 features including: surfer-level interference propensity (some surfers are chronically aggressive with priority), interference impact on heat outcomes (how often does the penalized surfer still win?), and whether interference-prone surfers represent a systematic risk factor that should adjust their predicted win probability. This is a relatively small data engineering task during collection but could yield a unique edge if interference patterns are predictable and not priced into market odds.

**Elo and rankings — a separate but related system:** The WSL ranking system (which determines tour qualification, seedings, and jersey assignments) is related to but distinct from the wave scoring system. Research and document how the WSL ranking points system works: how points are awarded per event based on finishing position, how the season standings are calculated, whether there's an official Elo-like system or just cumulative points, and how the ranking system has changed over time. This matters because rankings influence seedings (which affect who faces whom in early rounds), and because rankings/points pressure may be a feature in the model (a surfer fighting for qualification has different incentives than one already safely qualified). Note: "Elo" in the context of this project will also refer to our own Elo rating system built during modeling (Phase 6), which is distinct from the WSL's official ranking system. Be precise about which system is being referenced at all times to avoid confusion.

**WSL CT Ranking Points Table (confirmed from official WSL Rule Book and event results pages):**

| Finishing Position | Round Eliminated | Ranking Points |
|--------------------|-----------------|----------------|
| 1st | Winner | 10,000 |
| 2nd | Final (runner-up) | 7,800 |
| Equal 3rd | Semifinal | 6,085 |
| Equal 5th | Quarterfinal | 4,745 |
| Equal 9th | Round of 16 | 3,710 |
| Equal 13th | Round of 32 | 2,890 |
| Equal 17th | Elimination Round (1st/2nd) | ~2,255 |
| Equal 25th | Opening Round (top advancers to Elim) | ~1,760 |
| Equal 33rd | Opening Round (last place, eliminated) | ~1,000 |

Note: Opening Round and Elimination Round points have historically varied by year and are not always documented publicly with the same precision as R32+ positions. The 2026 format eliminates Finals Day; instead, the final two events (Cloudbreak and Pipe Masters) carry a **1.5x points multiplier** — winner earns 15,000 points. All points values above apply to standard regular-season events; confirm exact 2026 values against the official 2026 WSL Rule Book (Appendix B). Men's and women's events award identical points.

**Ranking feedback loop mitigation (added post-Phase 0 review):** Rankings influence seedings, which influence matchups, which influence outcomes, which influence rankings — a feedback loop. This creates endogeneity: ranking is both predictor and outcome. Specific mitigation approaches to evaluate during Phase 5-6:

1. **Instrumental variable approach:** Use lagged rankings (e.g., ranking at the START of the season) as an instrument that predicts current seeding but is not directly caused by current performance.
2. **Two-stage modeling:** First model predicts seeding/draw from rankings; second model predicts heat outcome conditional on draw. This decomposes the ranking effect into "who you face" (structural) vs. "how good you are" (skill).
3. **Elo as the primary skill variable instead of ranking:** Because Elo adjusts for opponent strength and is updated heat-by-heat, it better captures "true skill" without the feedback contamination of points-based rankings. Use Elo as the skill input; use ranking only as a feature capturing seeding effects and pressure.
4. **Within-event detrending:** Within a single event, rankings are fixed. This creates a quasi-natural experiment: the ranking is determined before the event starts, so within-event variation in opponent quality comes from the bracket draw, not from ranking changes. Exploit this by treating within-event heats as "fixed-ranking" observations.

The recommended approach is #3 (Elo as primary, ranking as structural/pressure feature) with #4 for robustness. Document the choice in the Decision Log during Phase 6.

**Project Elo system — formula and design intent (added post-Phase 0 review):**

The project's Elo system is separate from WSL rankings and will be built from scratch during Phase 6. The core formula:

```
New_Elo_A = Old_Elo_A + K × (Actual_A - Expected_A)
```

Where:
- `Expected_A = 1 / (1 + 10^((Elo_B - Elo_A) / 400))` — the logistic function giving the expected probability of A beating B based on rating difference
- `Actual_A = 1` if A wins, `0` if A loses
- `K` is the update factor (controls how quickly ratings change). Typical starting values: K=32 for new surfers (ratings change fast), K=16 for established surfers (ratings change slowly). The optimal K-factor will be tuned empirically.
- Starting Elo for all surfers: 1500 (arbitrary anchor point; only differences matter)

Design choices to resolve during Phase 6:
- **K-factor schedule:** Should K decay with number of rated heats (like Glicko's RD) or be fixed?
- **Surface-type conditioning:** Should Elo be split by break type (reef Elo, beach break Elo, point break Elo)?
- **Recency weighting:** Should older results decay? If so, at what rate?
- **3-person heat handling:** For pre-2019 data, how to credit a 2nd-place finish in a 3-person heat? Options: treat as half-win (0.5), treat as loss (0), or use a 3-player Elo extension.
- **Glicko-2 alternative:** Glicko-2 adds a "rating deviation" (confidence interval on the Elo estimate) and a "volatility" parameter (how erratic the surfer is). This naturally handles inactivity (RD increases when a surfer doesn't compete) and may be worth implementing alongside basic Elo for comparison.

**WSL Ranking vs. Project Elo — comparison and divergence analysis (added post-Phase 0 review):** A specific validation step during Phase 8: compare the project Elo rankings against the official WSL rankings for each season. Compute a divergence index (e.g., Kendall's tau rank correlation) and identify the surfers where Elo and WSL rankings disagree most. These divergence points are interesting for two reasons: (a) they reveal which surfers are over- or under-valued by a points-based system vs. an opponent-adjusted system, and (b) if the betting market prices based on WSL ranking rather than true skill (as proxied by Elo), the divergence cases are exactly where the model is most likely to find edge.

**Fantasy draft / total surfer value market exploration (added post-Phase 0 review):** If the Elo system produces meaningfully different rankings than WSL official rankings, there may be an additional market beyond direct heat betting: fantasy surf leagues (e.g., WSL Fantasy Surfer, third-party fantasy platforms). If the model identifies surfers whose "total event value" (expected points across all rounds) is systematically underpriced relative to their draft position or fantasy cost, this could be a lower-risk, higher-volume application of the model. Research during Phase 9 whether fantasy surf markets exist with real-money stakes, and whether the model's surfer valuations translate into fantasy draft edge. This is a secondary monetization path, not a replacement for direct betting.

**Tour structure:** Championship Tour vs. Challenger Series vs. Qualifying Series. Number of events per season and how that's changed. Wildcard entries, injury replacements, and how the field is composed at each event.

**Why this matters for modeling:** Every structural change listed above represents a potential data discontinuity. If you train a model on data spanning a format change without accounting for it, the model is learning from two different sports simultaneously. You need to know where the break points are before you design your data schema.

**2026 as the structural target for live/near-live betting (added post-Phase 0 review):** The 2026 format — with higher-seeded surfers starting with priority in 2-person sudden-death heats — is the structure we are building toward. While historical data from earlier formats is valuable for learning surfer skill and tendencies, the model's deployment context is 2026+ events. This has specific implications for the modeling pipeline:

1. **Priority as a first-class model input:** In 2026, seeding determines who starts with priority. Priority determines wave access. This chain (seeding → priority → wave access → score → outcome) must be explicitly modeled, not treated as noise. The model needs a "priority advantage" parameter that can be estimated from historical data where priority information is available.
2. **Live betting integration:** Given the 2026 format's real-time nature (sudden-death heats where momentum shifts rapidly), the model should be designed from the start to support live or near-live probability updates. This means: (a) the model must accept mid-heat state as input (current scores, waves caught, priority status, time remaining), (b) probability estimates should update in real-time, and (c) the betting strategy (Phase 9) should include a live betting component alongside pre-event betting.
3. **Historical data mapping:** Most historical data does NOT have priority information. The model should learn the general skill/matchup relationships from all historical data, then apply the priority adjustment as a separate layer estimated from 2022+ data where seeding-to-priority mapping is known. This is a transfer learning approach: learn skill from depth, learn priority effects from recency.
4. **Logical mapping document:** Before Phase 5, produce a written document mapping how each historical format era (2010-2018 three-person heats, 2019-2021 transition, 2022-2025 with mid-season cut, 2026+) translates to the 2026 prediction context. Which features carry over? Which are era-specific? What adjustments are needed? This prevents ad hoc decisions during feature engineering.

### 0.2 — Generate Domain Hypotheses

Before looking at any data, write down what you believe drives outcomes. This is the foundation of the causal approach you want. You're not looking for correlations yet — you're building a causal theory of "what makes a surfer win a heat."

**Causal graph framework:** Structure these hypotheses as formal causal graphs, following the Judea Pearl DAG (Directed Acyclic Graph) framework. For each hypothesized causal relationship, draw the directional arrow (X → Y means "X causes Y") and explicitly identify potential confounders, mediators, and colliders. The benefit of this formalism is that it forces you to be precise about what you think causes what, and it makes confounding variables visible rather than implicit. For example: does "break familiarity" cause better scores directly, or does it cause better wave selection, which then causes better scores? The causal graph makes this distinction explicit, and it changes how you'd design features. Research and compare the Pearl DAG framework against other causal structure options (structural equation models, potential outcomes / Rubin causal model, do-calculus) and recommend the best fit for this project. The DAGs seem like the right tool given the domain's complexity, but surface alternatives so the choice is deliberate.

Organize hypotheses into categories:

**Surfer-level:** Career form, recent form, head-to-head record, age, experience, injury status, stance (regular vs. goofy and how that interacts with wave direction), style (power vs. progressive/aerial and how that interacts with wave type).

**Condition-level:** Wave size, wave type (beach break, point break, reef break, slab), swell direction, swell period, wind, tide, water temperature.

**Interaction effects:** Does surfer A's advantage over surfer B change depending on conditions? (e.g., a power surfer might dominate at Teahupoo but lose to an aerial surfer at Trestles). Does break familiarity matter — has a surfer competed at this specific break many times before?

**Situational:** Round of competition (do some surfers peak in later rounds?), season standings pressure (fighting for qualification vs. comfortably qualified), home-break advantage, mental/momentum factors (coming off a win vs. a loss).

The purpose of writing these down first is to prevent the data from "telling" you a story that isn't causal. When you later see a correlation in the data, you can check it against your prior hypotheses. If the data confirms a pre-registered hypothesis, that's stronger evidence than finding a pattern you weren't looking for.

**Intuitive multicollinearity pre-flag:** While constructing the causal graph, explicitly flag variable pairs that are likely to be highly correlated with each other — not because correlation is always a problem, but because undetected multicollinearity can inflate standard errors, make coefficients unstable, and obscure which variables are actually doing the causal work. Examples to consider: career win rate and WSL ranking (both capture "how good is this surfer" but from different angles), recent form and season points standing (both reflect recent performance), swell height and wave face size (closely related but not identical), age and years on tour (highly correlated but potentially different causal mechanisms — physical decline vs. experience). For each flagged pair, note on the causal graph why they might be collinear and which one you hypothesize is more causally direct. This pre-flagging exercise will be validated with statistical tests during Phase 5 (Feature Engineering), but having the intuitive map first prevents the statistical tests from being the only safeguard.

### 0.2b — Understand How Surf Betting Odds Are Generated

This is a distinct sub-step because understanding how odds are set is essential for knowing where inefficiency might exist. Research and document: how do sportsbooks set their surf odds? Is it primarily driven by a surfer's ranking or Elo-equivalent? Is there a human oddsmaker making qualitative judgments? How much does public betting action move the line? Are the odds set by a sophisticated model, or are they relatively crude? Understanding the odds-generation process tells you where the market's "blind spots" are likely to be — for example, if odds are primarily rank-based, a model that incorporates conditions and matchup effects might find edge in situations where a lower-ranked surfer is actually favored by the conditions. Conversely, if the oddsmaker already incorporates conditions, that particular angle may not provide edge. This sub-step directly informs Phase 9 (Betting Algorithm Research) and should be documented as part of the Phase 0 reference.

### 0.3 — Assess Market Efficiency

This is the viability question. Research and think carefully about:

**How deep is the surf betting market?** Niche sports markets tend to be less efficient than major sports (NFL, NBA, soccer), which is potentially good news. But they also have less liquidity, wider spreads, and fewer platforms — which affects how much you can bet and at what cost.

**Who else is modeling this?** If sophisticated quant shops are already in this market, the edge available to an individual with public data may be minimal. If the market is mostly recreational bettors, there's more room.

**What is the vig/juice?** The sportsbook's margin (the difference between true probability and implied probability from odds) is the hurdle your model must clear. If the vig is 10%, your model needs to be more than 10% better than the market to break even.

**Are there specific bet types where edge is more likely?** Heat-winner bets might be more efficient (more attention, simpler market) than outright tournament winner bets or podium bets, or vice versa. Understanding where the market is "thinnest" helps you focus.

### 0.4 — Platform Landscape Assessment

Research which platforms offer surf betting, what market types they support, and critically, how they handle winning bettors. This directly shapes your Phase 9–12 strategy.

**Sportsbooks vs. Exchanges:** Sportsbooks (like most traditional platforms) take the other side of your bet and therefore lose when you win — they have a direct incentive to limit sharp bettors. Exchanges (like Betfair) are peer-to-peer and take a commission regardless of who wins — they're structurally more tolerant of winners. This distinction is fundamental to your platform strategy.

**Prediction Markets:** You mentioned Kalshi. Research whether prediction markets would allow surf-related markets and what the constraints are (market creation process, liquidity, fees, regulatory status).

**The "getting limited" problem:** Understand the specific mechanisms — closing line value tracking, automated flagging, manual review triggers, stake reduction vs. full account closure — so that Phase 12 can incorporate real countermeasures, not just vague caution.

### 0.4b — Market Creation Contingency Assessment

🔴 **CONDITIONAL FORK:** If research in 0.4 reveals that the betting market for surf is severely limited — too few platforms, insufficient bet types, no exchange availability, or the available markets are too thin to generate meaningful returns — then this sub-step activates.

Explore thoroughly whether creating betting markets is a viable alternative path. This includes: Can markets be proposed or created on platforms like Kalshi, Polymarket, or Manifold? What is the process for creating a new market on each platform? What are the regulatory requirements? What fees apply? How would liquidity be generated — is there a way to attract counterparties, and is it realistic for a niche sport?

This is a fundamentally different problem than the core plan. The prediction model and the causal analysis don't change, but generating demand for a market you've created is a marketing, product, and community challenge, not a modeling one. If this path looks promising, pause here and conduct a dedicated assessment covering: target audience (who would bet on surf?), liquidity bootstrapping strategies, cost structure, and whether the potential returns justify the additional effort. The outcome should be a written brief that either (a) confirms this path is viable and outlines the additional work required as a supplement to this plan, or (b) determines it's not viable and documents why, so you don't revisit it.

The core plan (use a model to find edge and place bets) shouldn't change regardless of which platform path you take — but the go-to-market strategy and the Phase 11–12 execution steps would need significant revision if you're creating markets rather than betting into existing ones.

### Phase 0 Gate

🔴 **HUMAN GATE:** Before proceeding, you should have written answers to these questions:

1. Do I understand the sport's structure well enough to know where structural breaks exist in the data — for both the men's and women's circuits?
2. Is there a plausible reason to believe the surf betting market is inefficient enough to exploit?
3. What are the 2–3 most promising bet types / market segments to target?
4. What platforms are available, and what constraints do they impose? If platforms are limited, has the market creation contingency (0.4b) been evaluated?
5. Do I understand how odds are generated for surf, and does that understanding point to specific types of inefficiency?
6. Is the causal graph drafted, with confounders and multicollinearity pre-flagged?
7. Am I still motivated to continue given what I've learned?

If the answer to #2 is "no" or "I can't tell," that's not necessarily a deal-breaker — but you should acknowledge you're proceeding on faith rather than evidence, and set a hard stop at Phase 10 where you'll re-evaluate.

🔵 **RE-ANCHOR CHECK:** Before moving to Phase 1, re-read the domain reference document produced in this phase and confirm it's complete. This document will be referenced at every subsequent phase transition.

---

<a name="phase-1"></a>
## Phase 1: Data Landscape Audit

**Purpose:** Inventory all available data sources before designing any infrastructure. Know exactly what you have to work with.

🔵 **RE-ANCHOR CHECK:** Re-read Phase 0 domain reference. Confirm the data sources you're about to audit align with the causal hypotheses and domain structure documented there.

### 1.1 — Identify All Relevant Data Sources

Note: this step is intentionally titled broadly. Do not constrain the search to competition results only — condition data, environmental data, video data, surfer biometric/training data, social media sentiment, travel schedules, and any other potentially relevant source should be inventoried here, even if its usefulness isn't yet confirmed. The point is to cast a wide net now and narrow later based on evidence, rather than narrowing prematurely based on assumptions.

For each source you find, document: what data it contains, how far back it goes, what format it's in, how to access it (API, scraping, download), and what its limitations are. Specifically look for:

**Official WSL data:** worldsurfleague.com result pages, any official APIs or data feeds, historical event archives. Pay particular attention to whether there is a structured API — in previous work, obvious API routes were missed. For every data source, actively probe for API access (check developer documentation, inspect network requests on the website, look for GraphQL endpoints, search for third-party API wrappers on GitHub). If it *seems like* there should be an easier access method than what you've found, keep digging — there usually is.

**Third-party datasets:** Kaggle datasets, GitHub repos, academic datasets, surf analytics blogs or sites that have compiled data.

**Ocean and environmental condition data:** This category is critically important and deserves a detailed treatment of not just *what* data exists but *how and when* each source would be used in the prediction pipeline. Sources to investigate include: Surfline (forecasts and historical observations), Magic Seaweed, NOAA buoy data, Windy, local weather stations near WSL venues, and any other oceanographic or meteorological data sources. For each source, document:

- What it measures (swell height at buoy vs. wave face height at shore, offshore wind vs. onshore wind, etc.)
- The difference between forecast data and observed data, and which is available when (before the event for forecasting, during/after for validation)
- Temporal resolution (hourly? every 30 minutes? daily averages?)
- Spatial resolution (how far is the nearest buoy from the actual break?)

**The buoy-to-shore calibration problem:** This is a meaningful sub-problem that will likely require its own modeling effort. Buoy data (swell height, period, direction) measured offshore is not the same as what a surfer experiences at the break. The wave transforms as it travels from the buoy to the shore — bottom contour, refraction, local wind effects, and tide all modify the wave. To use buoy/forecast data for prediction, you need either (a) a physics-based or empirical model that translates buoy readings to shore conditions at each specific break, or (b) historical data that lets you calibrate the relationship between buoy readings and actual competition conditions. This calibration would include confidence intervals — how much uncertainty exists between buoy reading X and actual wave face Y at break Z? Those confidence intervals should carry through to the competition prediction model as input uncertainty. Plan for this as a sub-step within Phase 5 (Feature Engineering), with its own gate. It may expand the scope significantly but should meaningfully improve condition-dependent predictions.

**Betting odds data:** Historical odds archives (odds-portal, oddsportal.com for surf), NXTbets if they have historical data, any other sources of what the odds were at the time of past events.

**Walled-garden data flag:** During research, if you encounter indications that valuable data exists but is not publicly accessible — for example, inside the WSL's internal systems, behind Surfline's premium paywall, proprietary to specific analytics companies, or held by betting platforms — document it explicitly. Even if you can't access it now, knowing that it exists gives you a mental accounting of what the market participants with the most resources might be using, and it flags potential future data acquisition opportunities (premium subscriptions, partnerships, FOIA-style requests for public environmental data, etc.).

### 1.2 — Assess Data Completeness & Quality

For each source, before committing to use it, evaluate:

**Coverage:** Does it cover all events, or only some? Are there gaps in years or specific events?

**Granularity:** Heat-level results? Wave-by-wave scores? Or only final event placements? (Wave-level data would be far more powerful but may not be available historically.)

**Consistency:** Has the format changed over time? Are field names consistent? Are there encoding issues?

**Reliability:** Is this an official source or a fan-compiled dataset? How many errors should you expect?

**Video data — a planned future data source:** Competition video is a potentially rich data source that is unlikely to be captured in structured datasets. Before dismissing it as too expensive or complex, identify what video-derivable features might be valuable and not available from any other source. Candidates include: paddling strength/fitness (how quickly a surfer paddles into waves — a proxy for physical condition), wave selection patterns (does a surfer tend to pick off smaller, safer waves or wait for bigger, riskier ones?), riding style classification (cautious vs. aggressive, aerial-focused vs. power-focused, signature moves like Caroline Marks' backhand turns), positioning and priority strategy (how a surfer uses the priority system tactically), and the time between waves (which could indicate fatigue or strategic patience). The plan for video is not to build a computer vision pipeline now, but to: (a) catalog what video-derived features would be most valuable, (b) determine how much video would be needed for a representative sample (likely a set of heats per surfer across multiple wave types, not exhaustive footage), (c) determine whether timestamp markers exist for each wave within a heat's video recording (this would dramatically reduce the amount of video that needs to be reviewed by pinpointing exactly when each wave occurs), and (d) scope the effort as a potential Phase 5b sub-project for manual or semi-automated feature extraction. This keeps the door open without derailing the current plan.

**Video analysis — aerial risk/reward sub-workstream (added post-Phase 0 review):** Within the video analysis scope, aerial maneuvers deserve specific, dedicated attention due to the asymmetric risk/reward profile: airs score ~2.3 points higher than traditional maneuvers when completed (~7.40 vs. ~5.08 avg), but completion rates are only ~45.4% vs. ~90% for traditional maneuvers. This creates a unique modeling opportunity:

- **Per-surfer aerial propensity:** How often does each surfer attempt airs in competition? This varies enormously and is a proxy for style and risk appetite.
- **Per-surfer aerial completion rate:** How often do they land? A surfer who attempts airs frequently but lands only 30% is a different risk profile than one who attempts rarely but lands 70%.
- **Condition-dependent aerial viability:** Airs are more feasible in certain wave types (open-face, lighter winds) and less in others (heavy barrels, strong onshore). The interaction between surfer aerial propensity and condition suitability for airs could be a powerful predictive feature.
- **Women's vs. men's aerial gap:** Research indicates aerial attempts and completions are "extremely rare" in women's competition. This is a current structural difference that may evolve — tracking it over time reveals whether the women's tour is converging toward the men's aerial emphasis.

This is a standalone workstream because it requires video review (airs aren't tagged in structured data) and should be scoped incrementally: start with 50-100 heats across a range of surfers and conditions, classify each wave as aerial attempt or traditional, record completion, and build the per-surfer aerial profile. This is labor-intensive but is precisely the kind of data that the market (ALT Sports Data, generic oddsmakers) is unlikely to incorporate — making it a candidate for genuine edge.

**Data scarcity acknowledgment:** Surfing analytics is a much less developed field than analytics for major team sports. Expect data to be scarce, inconsistently formatted, and spread across many small sources. The research posture for this step should be to scour far and wide — niche surf forums, academic theses from sports science programs, surf coaching platforms, fantasy surf league data, international surfing association archives, and historical competition programs/media guides. Don't assume that because a centralized database doesn't exist, the data itself doesn't exist somewhere.

### 1.3 — Assess Betting Odds Data Availability

This is often the hardest data to find for niche sports. Specifically determine:

**Can you get historical odds?** Without historical odds, you can't backtest your betting algorithm against what was actually available. You can still test the predictive model, but you can't simulate realistic betting performance. This is a critical constraint — if no historical odds exist, Phase 10 (backtesting) will need to simulate odds from implied probabilities, which introduces significant uncertainty.

**At what granularity?** Opening odds, closing odds, odds movement over time? The closer to closing odds you can get, the more realistic your backtest.

**From which platforms?** Different platforms offer different odds on the same event. Understanding which platform's odds you're backtesting against matters.

**Contingency plan: odds collection from upcoming events.** Historical odds for surf are likely sparse or nonexistent. If this proves true, the plan should include an intentional forward-looking odds collection phase: beginning with the next available WSL event, systematically capture odds as they are posted (from NXTbets, any sportsbooks, prediction markets) and track how they move over time. This creates a growing odds dataset that you can use for future backtesting, even if historical odds remain unavailable. Treat this as an ongoing parallel workstream that starts as soon as platforms with odds are identified in Phase 0.4.

**NXTbets blog odds as MVP seed data.** You've identified that NXTbets has published some odds in long-form blog content. These may not be comprehensive, but they could provide enough data points for an MVP-level exercise: take the surfers and odds mentioned in those blogs, generate predictions from the model, and compare. This gives you an early, rough signal on whether the model's probability estimates are in the right ballpark relative to the market's — long before you have a full odds dataset. Plan this as an explicit early validation step in Phase 8 (Model Testing), where a small set of predictions is compared against NXTbets-derived odds.

**Live event calibration cycle.** Because historical odds are likely limited, plan for a series of live event validation cycles: for each upcoming event where odds are available, the model generates pre-event predictions, actual odds are recorded, the event plays out, and results are compared. This isn't paper trading yet (Phase 11) — it's model calibration. You'll likely need several events (perhaps 3–5 at minimum) to have a meaningful calibration sample, so this should begin as early as possible. Acknowledge that this introduces a time dependency into the project — you can't rush this step, because events happen on the WSL's schedule, not yours.

### 1.4 — Gap Analysis

After inventorying everything, explicitly document: what data do I wish I had but can't find? This is important because it sets realistic expectations for what the model can do. For example, if you can't find historical surf condition data matched to specific heats, then condition-based features won't be available — and your model will be limited to surfer-level and matchup-level features only. That's fine, but you need to know it upfront rather than discovering it mid-build.

### Phase 1 Gate

🔴 **HUMAN GATE:** Review the data inventory and answer:

1. Is there enough historical competition data (at least several seasons of heat-level results) to train a model?
2. Is there condition data that can be matched to specific events/heats? If so, what's the plan for buoy-to-shore calibration?
3. Is there historical odds data, and if not, is the forward-looking odds collection plan in place?
4. What's my "minimum viable dataset" — the smallest set of features that could plausibly predict outcomes — and can I actually obtain it?
5. Have walled-garden data sources been flagged?
6. Has every data access route been thoroughly checked, especially for APIs that might exist but weren't immediately obvious?

If the answer to #1 is "no," the project may not be viable in its current form. Consider whether alternative data (wave-by-wave video analysis, manual data entry from archived broadcasts) could fill the gap, and whether that's worth the effort.

🔵 **RE-ANCHOR CHECK:** Re-read Phase 0 domain reference and this master plan. Confirm that the data sources identified align with the causal hypotheses. Are there hypothesized causal variables for which no data source was found? Flag these explicitly.

---

<a name="phase-2"></a>
## Phase 2: Architecture & Schema Design

**Purpose:** Design the data model, ETL pipeline, storage approach, and technical stack before writing collection code. This is the blueprint.

### 2.0 — Git Repository Setup

Before any design work begins, set up the project repository. GitHub username: `cowabungasurfbetco`. Create the repo, establish a branching convention (recommendation: `main` for stable/reviewed work, `dev` for active work, feature branches for specific phases), set up a basic README, and confirm push/pull access works. This plan document itself should be the first committed file. From this point forward, all code, configuration, data schemas, and documentation are version-controlled.

🔴 **HUMAN GATE:** Confirm Git access is working — you can clone, commit, push, and pull.

### 2.1 — Entity-Relationship Design

Based on what you learned in Phase 1 (not what you wish you had), design your data schema. The core entities will likely include:

**Events:** Event name, year, location, break name, break type, tour level, dates, format details, circuit (men's or women's).

**Heats:** Event ID, round, heat number, surfers involved, heat result, conditions during the heat (if available).

**Surfers:** Surfer ID, name, stance, nationality, birth year, career start year, circuit.

**Waves (all waves, not just the best two):** Heat ID, surfer ID, wave number, wave score, timestamp within heat (if available), whether this wave counted toward the surfer's heat total. Capturing all waves — not just the two highest-scoring — is important for several reasons. The non-counting waves reveal information about wave selection strategy, consistency vs. volatility of scoring, risk appetite (did the surfer attempt big moves on "throwaway" waves?), and fatigue patterns within a heat. These may not be immediately useful features, but having the data makes them available for exploratory analysis and visualization. If you can only see the top-two scores, you're missing the process that produced them. Additionally, if timestamp markers for each wave exist within heat video recordings, capture those in this table — they would make video analysis (Phase 5b candidate) dramatically more efficient by pinpointing exactly when each wave occurs in the footage.

**Conditions (if available):** Event ID (or heat ID if that granular), swell height, swell period, swell direction, wind speed, wind direction, tide.

**Buoy/Forecast Data (if pursuing the buoy-to-shore calibration):** Buoy ID, timestamp, swell height, swell period, swell direction, associated break, along with the shore-observed conditions for calibration.

**Odds (if available):** Event ID, heat ID, surfer ID, bet type (heat win, event win, podium), odds value, timestamp of odds, platform.

Key design decisions to make explicitly:

**Granularity:** Is your primary unit of analysis the heat, the event, or the surfer-event? This affects everything downstream. (Recommendation: design at the most granular level available — waves if possible, otherwise heats — and aggregate up as needed.)

**How to handle format changes:** The 3-person to 2-person heat transition, Finals Day introduction, and mid-season cut are all structural changes. Do you include pre-change data with a flag? Exclude it? Model it separately? This is a design decision that should be made here, not discovered during modeling.

**Time-awareness:** Every piece of data needs a timestamp. This is critical because your model must never see future data during training — and it's surprisingly easy to accidentally leak future information (e.g., using a surfer's end-of-season ranking to predict a heat that happened mid-season).

### 2.2 — ETL Pipeline Design

Map out the flow: where data comes from → how it's extracted → how it's transformed → where it lands.

For each data source, document: access method (API call, web scrape, file download), frequency of update (one-time historical pull vs. ongoing), transformation steps needed (parsing, joining, cleaning), and where the output goes.

**Include a staging layer.** Raw data should land in a "raw" zone first, untransformed. Transformations produce a "clean" zone. This way, if your transformation logic has a bug, you can re-run it from raw without re-collecting.

### 2.3 — Tech Stack Decisions

For a personal-use project, keep it simple but intentional:

**Storage:** SQLite is fine for a personal project (single file, no server, SQL-capable). PostgreSQL is overkill unless you need concurrent access. CSV/Parquet flat files work too, but you lose the ability to do relational queries easily.

**Language:** Python is the natural choice given the ML ecosystem. Decide on notebook-based (Jupyter) vs. script-based workflow, or a hybrid.

**Environment:** How will you manage dependencies? (A simple requirements.txt or conda environment is fine, but it needs to exist from the start.)

**Version control:** Git, already established in 2.0.

### 2.4 — Logging & Audit Trail Design

Design this now, not as an afterthought. Every pipeline run should log: when it ran, what data it processed, what the row counts were before and after, and whether any quality checks failed. This doesn't need to be complex — even writing to a log file or a "pipeline_runs" table is sufficient — but it must exist.

### Phase 2 Gate

🔴 **HUMAN GATE:** Review the ERD, the ETL flow diagram, and the tech stack decisions. Specifically verify:

1. Does the schema accommodate the data sources identified in Phase 1?
2. Is there a clear staging/raw layer before transformation?
3. Are format changes and structural breaks accounted for in the schema?
4. Is every entity time-stamped to prevent future-data leakage?
5. Does the waves table capture all waves (not just the top two)?
6. Is Git set up and working?
7. Are you comfortable with the tech stack choices?

---

<a name="phase-3"></a>
## Phase 3: Data Collection — Small Scale Pilot

**Purpose:** Collect a small, inspectable sample of data to verify that your ETL works correctly before scaling up. This is where you catch the mistakes that would otherwise propagate through the entire dataset.

🔵 **RE-ANCHOR CHECK:** Re-read the Phase 0 domain reference and the Phase 2 schema. Confirm the pilot collection will exercise all the entities and edge cases defined in the schema, including structural break handling.

### 3.1 — Choose a Pilot Scope

Select a narrow slice of data — something like one full season of CT events, or 2–3 events with complete heat-level data. The goal is small enough to manually inspect but large enough to exercise all the edge cases in your pipeline (different rounds, different heat sizes, wildcards, injuries, walkovers).

### 3.2 — Run the ETL

Execute your collection and transformation pipeline on just this pilot scope.

### 3.3 — Inspection & Validation

This is the most important part of the pilot. Produce and review:

**Row counts:** How many events, heats, surfers, scores did you get? Do these match what you'd expect from manually checking the WSL website for that season?

**Sample records:** Look at 10–20 individual records end-to-end. Pick a specific heat you can verify manually — look up the actual result on the WSL site and confirm your data matches.

**Distribution summaries:** Heat scores (are they in a reasonable 0–20 range?), number of heats per event (does it match the expected bracket size?), number of surfers per heat (all 2, or are there some 3-person heats from the older format?).

**Null/missing analysis:** What percentage of fields are null? Which fields? Is that expected?

**Visualizations:** Score distributions, surfer appearance counts, events-per-year histogram. These aren't for modeling yet — they're for catching data problems that summary statistics miss.

🟡 **AGENTIC CHECK:** Generate an automated Data Quality Report for the pilot data. This report should include completeness percentages per field, value range checks, duplicate detection, referential integrity (does every heat reference a valid event? does every score reference a valid surfer?), and any outlier flags. You review the report.

### 3.4 — Known-Answer Testing

This is a specific validation step that catches a surprisingly common class of errors: pick 5–10 heats where you already know the result (from manually looking it up), and confirm your data has the correct winner, correct scores, and correct round assignment. This catches misaligned data, off-by-one errors in heat numbering, and surfer ID mismatches.

### Phase 3 Gate

🔴 **HUMAN GATE:** You must personally inspect the pilot data and confirm:

1. Sample records match what's on the WSL website.
2. Row counts are plausible.
3. Score distributions look reasonable.
4. No systematic nulls or missing data in critical fields.
5. Known-answer tests all pass.

Do not proceed to full-scale collection until you are confident the pilot data is correct. Any error found here will be multiplied across the full dataset.

---

<a name="phase-4"></a>
## Phase 4: Data Collection — Full Scale

**Purpose:** Collect the complete historical dataset using the validated pipeline from Phase 3.

### 4.1 — Execute Full Collection

Run the ETL for all available historical data. If the collection spans format changes (identified in Phase 0), make sure the pipeline handles each era correctly and flags which era each record belongs to.

### 4.2 — Automated Quality Checks

🟢 **AUTOMATED GUARD:** Run the same DQ report from Phase 3 on the full dataset. Flag any records that fall outside expected ranges, any events with unexpected heat counts, any surfers appearing in events they shouldn't be in, or any time periods with suspiciously sparse data.

🟡 **AGENTIC CHECK:** Generate a comprehensive Data Profile Report including: records per year (to spot coverage gaps), completeness trends over time (older data may be sparser), cross-table consistency checks, and year-over-year comparisons of key distributions (average heat scores, field sizes, number of events).

### 4.3 — Human Spot-Check

Even with automated checks, manually verify 3–5 records from different eras (early data, mid-period, recent) to confirm consistency across time.

### 4.4 — Create a Data Dictionary

Document every field: name, type, source, description, valid range, known quirks. This is your reference for all downstream work. It should be updated whenever a field's interpretation changes or a new issue is discovered.

### Phase 4 Gate

🟡 **AGENTIC CHECK** + 🔴 **HUMAN SPOT-CHECK:**

1. Automated DQ report shows no critical issues.
2. Coverage matches expectations (right number of events per year, right number of surfers).
3. Spot-checked records from different eras are accurate.
4. Data dictionary is complete.

🔵 **RE-ANCHOR CHECK:** Re-read Phase 0 domain reference. Compare the data you've actually collected against the causal hypotheses. Are there hypothesized causal variables that turned out to have no data? Update the Assumptions Register.

---

<a name="phase-5"></a>
## Phase 5: Feature Engineering & Selection

**Purpose:** Transform raw data into model-ready features, guided by the domain hypotheses from Phase 0. This is where domain knowledge meets data.

🔵 **RE-ANCHOR CHECK:** Re-read Phase 0 causal graph. Every feature you create should map to a node or edge in that graph. If you find yourself creating features that don't correspond to any hypothesized causal pathway, pause and ask: is this a new hypothesis that should be added to the graph, or is this an unmotivated fishing expedition?

### 5.0b — Hypothesis Validation Visualizations (added post-Phase 0 review)

Before engineering features, produce exploratory visualizations to validate (or challenge) the Phase 0 domain hypotheses using the collected data. This is a pre-feature-engineering step that grounds the causal hypotheses in observed data patterns. For each hypothesis category:

- **Surfer skill/Elo:** Distribution of win rates across surfers. Is there a clear hierarchy, or is it more uniform? How stable are rankings year-over-year?
- **Venue/familiarity:** Score distributions by venue — do certain breaks produce consistently different scoring? Do surfers with more heats at a venue score disproportionately better?
- **Conditions:** Score distributions by reported wave size, wind conditions, break type. Do conditions affect scoring variance? Do upsets happen more in certain conditions?
- **Stance/frontside advantage:** Do regular-foot surfers perform differently at left-breaking waves vs. right-breaking? Split by stance and wave direction.
- **Season pressure:** Do surfers near the mid-season cut line (2022-2025 data) perform differently in events 4-5 vs. events 1-3?
- **Home advantage:** Win rates for surfers at venues in their home country/region vs. away venues.
- **Head-to-head:** For surfer pairs with 5+ matchups, are head-to-head records predictive of future matchups?

These visualizations should be reviewed before proceeding to feature engineering. They serve as both validation artifacts and exploratory tools that may reveal unexpected patterns worth adding to the causal graph.

🔴 **HUMAN GATE:** Review the hypothesis visualizations. Do the data patterns align with the causal hypotheses? Are there surprising patterns that suggest new hypotheses? Are there hypotheses that the data clearly contradicts? Update the causal graph as needed before proceeding.

### 5.0c — Market Model Reconstruction: Reverse-Engineering ALT Sports Data's Pricing Function (added post-Phase 0 review)

**Purpose:** Before engineering features for *our* model, understand what the *market's* model is already pricing. If ALT's odds-setting model is simple (primarily rank-based), then the gap between what they price and what actually drives outcomes defines our edge surface. If their model is sophisticated, the edge surface is smaller and we need to know that early.

**Why this matters strategically:** Every feature we engineer needs to add information *beyond what the market already knows.* If ALT already incorporates venue history, then our venue-familiarity features aren't adding edge — they're just matching the market. But if ALT is using rankings plus a thin recency adjustment and nothing else, then condition-surfer interactions, venue-specific performance, aerial propensity, and priority effects are all potential edge vectors the market is blind to. This step tells us where to aim.

**Hypothesis to test:** ALT's implied probabilities are primarily a function of WSL ranking (or a close proxy like season points), with possible secondary inputs including recent form (last 1-2 events) and head-to-head record. The model likely does NOT incorporate: wave conditions, break type, surfer style matchups, stance-wave direction interactions, or situational pressure variables. If this hypothesis is correct, the condition-surfer interaction thesis from Phase 0 represents the primary edge opportunity.

**Method:**

1. **Assemble the dataset:** For every heat where ALT-sourced odds are available (from the NXTbets odds collection in Phase 1 and any forward-collected odds), pair each surfer's implied probability (from the decimal odds) with their WSL ranking at that point in time, recent form metrics (last 3 events win rate, last event finish), head-to-head record against the specific opponent, and any other readily available surfer-level variables.

2. **Fit a simple reconstruction model:** Regress the implied probability against WSL ranking alone. Record the R². Then iteratively add variables — recent form, H2H record, venue history, seeding position — and track how much each addition improves R². The variable that produces the biggest R² jump after ranking is likely ALT's second most important input. If ranking alone yields R² > 0.85, the model is rank-dominant. If R² is < 0.65 with ranking alone, they're using more than just rankings.

3. **Analyze residuals:** The residuals from the best-fit reconstruction model represent information in the odds that ISN'T explained by the variables you tested. If residuals are small and random, you've captured essentially all of ALT's pricing logic. If residuals show systematic patterns (e.g., consistently off for certain surfers, at certain venues, or in certain conditions), those patterns reveal additional inputs ALT may be using — or biases in their model that represent exploitable inefficiency.

4. **Stability check:** Run the reconstruction separately for different time periods (e.g., 2023 events vs. 2024 vs. 2025) to detect whether ALT has updated their model over time. If the coefficients shift meaningfully across periods, they're iterating — which means any edge we find may be time-limited.

5. **Derive the "edge map":** Produce a matrix showing, for each feature category in our model (conditions, venue familiarity, style matchups, aerial propensity, situational pressure, priority effects), whether ALT appears to price that factor. Categories where ALT is NOT pricing the factor — but where our Phase 5.0b visualizations show the factor matters — are the primary edge targets. Categories where ALT IS pricing the factor should still be modeled (for accuracy), but are not edge sources.

**Deliverables:**
- A written report documenting the reconstruction model's fit, the R² at each variable addition step, residual analysis, and stability across time periods
- The "edge map" matrix showing priced vs. unpriced factors
- An updated prioritization of feature engineering efforts based on which features are most likely to produce genuine edge (i.e., features that predict outcomes but are NOT already reflected in the odds)

**Important caveats to document:**
- This reconstruction approximates ALT's model; it doesn't reveal their actual code or methodology. The approximation is sufficient for our purposes (identifying priced vs. unpriced factors).
- ALT can update their model at any time. The reconstruction should be re-run periodically (at minimum once per season, ideally after every 2-3 events) as an ongoing monitoring task during Phase 13 (Ongoing Monitoring).
- Sample size will be limited, especially early on. The reconstruction's confidence intervals matter — a reconstruction based on 50 heats has wide uncertainty. Don't over-interpret small R² differences until you have 200+ observations.
- If the reconstruction reveals ALT's model is MORE sophisticated than hypothesized (incorporating conditions, matchups, etc.), that's a critical finding that narrows the viable edge surface. This should trigger a re-evaluation of the project's viability at the Phase 5 gate.

**Ongoing market model monitoring (Phase 13 integration):**
The initial reconstruction in 5.0c produces a baseline snapshot of ALT's pricing function. But this is a living adversary, not a static target — ALT has every incentive to improve their model over time, especially if sharp bettors start exploiting the gaps we identify. The monitoring protocol:

1. **Coefficient drift detection:** After every 2-3 CT events, re-run the reconstruction regression on a rolling window (e.g., last 12 months of odds data). Compare the new coefficients and R² against the baseline. If the ranking coefficient drops meaningfully (say, from 0.92 to 0.75) while new variables become significant, ALT has likely added inputs — and the edge map needs to be redrawn.
2. **Residual pattern shift:** Track whether the systematic residual patterns identified in the initial reconstruction persist, shrink, or disappear. If our edge thesis is "ALT doesn't price conditions" and the residuals that previously correlated with wave size stop correlating, they've caught on. This is the most direct signal that a specific edge vector is closing.
3. **Prediction-vs-closing-line test:** For each event, record our model's predicted probability BEFORE odds are published, the opening ALT odds, and the closing odds. If the closing line moves toward our prediction more often than away from it, we're capturing information ALT eventually incorporates (a good sign for edge, but also a signal they're learning — possibly from the same information source). If the closing line stops moving toward us, either our model has degraded or theirs has improved.
4. **Trigger thresholds:** Define explicit thresholds for when a reconstruction update should trigger action. For example: if the R² of ranking-only reconstruction drops below 0.70 (meaning ranking alone no longer explains most of their pricing), trigger a full re-reconstruction. If any previously-unpriced factor shows up as significant in the new reconstruction, flag that edge vector as potentially closing and re-run the scenario model with revised edge assumptions.
5. **Annual full reconstruction:** Regardless of drift signals, run a complete reconstruction from scratch once per year using the full accumulated odds dataset. This catches gradual changes that the rolling window might smooth over.

This monitoring protocol should be written into Phase 13 (Ongoing Monitoring) as a formal recurring task with its own dashboard or summary report. The point isn't paranoia — it's that our entire betting strategy is premised on knowing what the market does and doesn't price, and that knowledge has a shelf life.

🟡 **AGENTIC CHECK:** After completing the reconstruction, verify that the R² and residual patterns are internally consistent (e.g., if ranking explains 90% of implied probability, then residuals should be small and uncorrelated with ranking). Flag any anomalies.

🔴 **HUMAN GATE:** Review the edge map. If ALT's model appears to already price the primary factors you were counting on for edge, this is a decision point: either identify alternative edge sources, accept a smaller edge, or reconsider the project's expected returns. Update the scenario model (Excel workbook) with revised edge assumptions if needed.

### 5.1 — Feature Candidate Generation

Using your Phase 0 hypotheses as a roadmap, generate candidate features. Organize them by category:

**Surfer form features:** Win rate (career, last N events, last N heats), average heat score, score variance, podium rate, trend (improving vs. declining form). Think carefully about the lookback window — too short and you get noise, too long and you miss form changes.

**Matchup features:** Head-to-head record between the two surfers in a heat, style matchup considerations, relative ranking.

**Break/condition features:** Surfer's historical performance at this specific break, performance in similar conditions (if condition data is available), break type and how the surfer's historical scores vary by break type.

**Situational features:** Round number, season point standings at time of heat, whether surfer is "at home" (break near their home country/region).

**Wave-level features (from all-waves data):** If all individual wave scores were captured, derive features such as: scoring consistency (standard deviation of wave scores within a heat), "ceiling" performance (highest single wave score over recent heats), wave selection efficiency (ratio of counting waves to total waves attempted), risk appetite (frequency of high-variance scoring attempts), and fatigue indicators (do scores decline within a heat?).

**Critical consideration — time-aware features:** Every feature must be computed using only information available BEFORE the heat it's predicting. For example, "win rate in the current season" must be computed using only events that occurred before the current event, not the full season's results. This is the most common source of data leakage in sports models and it can massively inflate apparent accuracy. Build this constraint into your feature engineering code from the start, and verify it with explicit tests.

### 5.1b — Buoy-to-Shore Calibration Model (Conditional)

If condition data and buoy data are available from Phase 1/4, this sub-step builds the calibration model that translates offshore buoy readings to onshore wave conditions at each specific break. This is essentially a small secondary model that feeds into the main prediction model. It should include: an empirical or physics-informed mapping from buoy swell readings to estimated wave face height/quality at each WSL venue, confidence intervals on that mapping (how much uncertainty exists?), and validation against actual competition-day conditions (if available). These confidence intervals should propagate into the competition model's condition features as input uncertainty rather than being treated as point estimates.

**Buoy-to-shore translation formula — explicit requirement (added post-Phase 0 review):** The calibration model must account for at least the following physical variables: swell height (Hs), swell period (Tp), swell direction (Dp), wind speed and direction, tide state, and bathymetric characteristics of each break. The translation is NOT a single formula — it varies by venue because bottom contour, reef shape, and exposure angle differ at every WSL break. The minimum viable approach is an empirical regression per venue: `wave_face_at_shore = f(Hs_buoy, Tp, Dp, wind, tide | venue)`, calibrated against reported competition-day wave heights from WSL heat data or Surfline observations. A more sophisticated approach would use shallow-water wave transformation physics (Snell's law for refraction, shoaling coefficients, wave breaking criteria), but the empirical approach may be sufficient given limited data. The deliverable is: for each WSL venue, a calibrated function that takes buoy/forecast inputs and outputs estimated shore wave conditions with confidence intervals.

🔴 **HUMAN GATE on 5.1b:** Review the calibration model's accuracy and confidence intervals. Are the confidence intervals tight enough to be useful, or is the buoy-to-shore mapping so noisy that condition features won't add value? This is a scope check — if the calibration is poor, it may be better to drop condition features entirely and rely on surfer and matchup features only, rather than adding noisy inputs that degrade the model.

### 5.2 — Feature Quality Checks

For each candidate feature:

🟡 **AGENTIC CHECK:** Compute summary statistics (mean, median, standard deviation, min, max, null rate). Visualize the distribution. Check for: features that are nearly constant (low variance, which means low predictive value), features with high null rates (may not be usable), features with extreme outliers (investigate — are they real or data errors?), features that are highly correlated with each other (redundancy).

**Multicollinearity validation:** This is where you validate the intuitive multicollinearity pre-flags from Phase 0.2. Compute pairwise correlations for all candidate features and produce a correlation matrix heatmap. For the specific pairs flagged in Phase 0 (career win rate vs. ranking, recent form vs. season standing, swell height vs. wave face, age vs. years on tour, etc.), compute variance inflation factors (VIF) and determine whether the collinearity is severe enough to cause problems for the chosen model type. For tree-based models, multicollinearity is less problematic but still obscures interpretability. For regression-based or Bayesian models, it can be a serious issue. The action here isn't necessarily to drop correlated features — sometimes both capture different aspects of a causal mechanism — but to document the collinearity, understand it, and handle it deliberately (e.g., via regularization, PCA, or choosing one feature and dropping the other with documented reasoning).

### 5.3 — Feature Sanity Check

This is a domain-knowledge check, not a statistical check. For each feature, answer:

**Is this feature causal or correlational?** (e.g., "surfer's historical score at this break" is plausibly causal — familiarity with a wave matters. "Surfer's jersey color" is spurious.) Reference the Phase 0 causal graph — does this feature correspond to a node with an incoming or outgoing edge?

**Could this feature be capturing something else?** (e.g., "number of events competed in this season" might just be a proxy for "not injured" or "high enough ranking to qualify for all events" — the real causal factor is fitness/ranking, not event count.)

**Is this feature available at prediction time?** If you're predicting before a heat starts, you can't use any information from that heat or future heats. But can you get the surf forecast? Is the heat draw published in advance?

**Are there features you expected to matter (from Phase 0) that you can't construct from available data?** Document these as known limitations.

### 5.4 — Feature Interaction Brainstorm

Think about whether features interact. For example: a surfer's overall win rate might not matter much on its own, but their win rate at reef breaks specifically might be highly predictive when the event is at a reef break. These interaction effects are where domain knowledge can add the most value, because a model might find the correlation but the interaction term helps it find the right one.

### 5.5 — Feature Importance Preview (Optional but Recommended)

Before building the full model, run a simple model (logistic regression or a single decision tree) on your features to get a rough sense of which features the model finds useful. This isn't your final model — it's a sanity check. If the model says "jersey color" is the most important feature, something is wrong with your data. If it says "career win rate" and "head-to-head record" are important, that aligns with domain knowledge and builds confidence.

### Phase 5 Gate

🔴 **HUMAN GATE:** Review the full feature set and ask:

1. Does every feature pass the "available at prediction time" test?
2. Are there features I expected to matter that are missing? Why?
3. Do the feature distributions look reasonable?
4. Am I comfortable that no feature contains leaked future information?
5. Has multicollinearity been validated against the Phase 0 pre-flags, and are the decisions about correlated features documented?
6. If the buoy-to-shore calibration was attempted (5.1b), is the calibration quality sufficient to justify including condition features?
7. Are there obvious features I haven't thought of? (Pause here and brainstorm — this is worth an hour of thinking.)
8. **Update the Assumptions Register:** Document what assumptions each feature embeds (e.g., "recent form is computed over last 5 events" assumes 5 is the right window).

---

<a name="phase-6"></a>
## Phase 6: Model Research & Selection

**Purpose:** Survey the modeling options, understand tradeoffs, and choose an approach (or multiple approaches to compare). This is a research phase — no training yet.

🔵 **RE-ANCHOR CHECK:** Re-read the Phase 0 causal graph and the Phase 5 feature set. The model you choose must be able to represent the causal relationships you hypothesized and work with the features you've built. If the causal graph implies interaction effects, the model needs to handle interactions. If the features include uncertain inputs (like condition forecasts with confidence intervals), the model ideally handles input uncertainty.

### 6.1 — Model Option Inventory

For the surf prediction task, research and document the following model families. For each, you want to understand: how it works conceptually, what its strengths and weaknesses are on your three criteria (accuracy, interpretability, scalability), and what assumptions it makes.

The categories to evaluate include (but don't limit yourself to):

**Paired comparison / ranking models** (Bradley-Terry, Thurstone): Models specifically designed for "who beats whom" type outcomes. They estimate a latent "strength" for each surfer and predict the probability of one surfer beating another based on the strength difference. Strengths: theoretically grounded for head-to-head competition, interpretable (each surfer gets a rating), naturally produces probabilities. Weaknesses: basic versions don't incorporate covariates (conditions, break type), may not capture interaction effects.

**Elo / Glicko rating systems:** Iterative rating systems where surfers gain or lose points based on wins and losses. Widely used in chess, increasingly in other sports. Strengths: simple, dynamic (captures form changes), interpretable, minimal data needs. Weaknesses: doesn't directly incorporate external covariates, sensitive to K-factor tuning, not inherently probabilistic without extension.

**Logistic regression:** Predict heat outcome as a binary variable from features. Strengths: highly interpretable, produces calibrated probabilities naturally, fast, easy to understand coefficients. Weaknesses: assumes linear relationships between features and log-odds, may miss complex interactions.

**Regularized regression (Ridge, Lasso, Elastic Net):** Logistic regression with penalties that prevent overfitting. Especially relevant given your small sample size.

**Tree-based models (Random Forest, XGBoost, LightGBM):** Ensemble methods that capture non-linear relationships and interactions. Strengths: handle interactions naturally, robust to outliers, often high accuracy. Weaknesses: less interpretable (though SHAP values help), risk of overfitting with small samples, don't inherently produce well-calibrated probabilities.

**Bayesian hierarchical models:** Estimate surfer abilities with uncertainty, allow partial pooling across similar surfers or conditions. Strengths: naturally handles small samples (via priors), quantifies uncertainty (critical for betting — you want to know not just "who's favored" but "how confident am I"), can incorporate domain knowledge through prior specification, causal structure can be encoded. Weaknesses: computationally slower, requires more statistical sophistication to specify and diagnose.

**Ensemble approaches:** Combine multiple model types to get the best of each. Strengths: often outperforms any single model, reduces model-specific biases. Weaknesses: more complex, harder to interpret, risk of overfitting the ensemble itself.

### 6.2 — Evaluate Against Your Criteria

For each model family, assess on a grid:

**Accuracy potential:** Given your data size, feature set, and the nature of surf prediction, how well is this approach likely to perform?

**Interpretability:** Can you understand *why* the model makes a specific prediction? This matters not just for debugging but for building trust — if the model says a surfer will lose and you can't understand why, will you trust it enough to bet on it?

**Causal vs. correlational:** Does the model's structure encourage causal reasoning (e.g., Bayesian models where you specify a causal graph) or purely correlational pattern-matching (e.g., gradient boosted trees)?

**Calibration:** Does the model naturally produce well-calibrated probabilities? (A model is "calibrated" if when it says 70%, the outcome actually happens ~70% of the time.) This is arguably the most important property for betting, because your betting algorithm will compare your model's probability to the implied probability from odds. If your probabilities are not well-calibrated, your bets will be systematically wrong.

**Robustness to small samples:** The WSL CT has ~10 events per year with ~30 surfers. This is a small dataset by ML standards. Models that perform well on small data (Bayesian approaches, Elo-style systems, regularized regression) deserve extra weight.

### 6.3 — Recommendation & Decision

Based on your research, document which approach (or combination) you plan to use, and why. If you plan to test multiple approaches head-to-head (recommended), specify which ones and what metric you'll use to compare them.

**A note on the causal emphasis:** You've asked for causal approaches over correlational ones. In practice, a reasonable strategy is to build a causal-ish model as your primary (e.g., Bayesian hierarchical model with a structure that reflects your domain hypotheses) and a correlational model as a challenger (e.g., XGBoost on the same features). If the correlational model significantly outperforms the causal one, that's a signal that either your causal model is mis-specified or there are important patterns you haven't accounted for. If they perform similarly, prefer the causal model because it's more robust to distributional shift and more interpretable.

### Phase 6 Gate

🔴 **HUMAN GATE:** Review the model comparison and confirm:

1. Do I understand each option well enough to explain why I'm choosing (or not choosing) it?
2. Is the chosen approach appropriate for my data size and feature set?
3. Does the chosen approach produce calibrated probabilities (or can it be calibrated post-hoc)?
4. Am I testing at least one causal and one correlational approach for comparison?
5. **Decision Log entry:** Record the choice and the reasoning.

---

<a name="phase-7"></a>
## Phase 7: Model Build & Training

**Purpose:** Implement and train the chosen model(s). This is where the previous phases pay off — if your data, features, and model choice are solid, this phase is relatively mechanical.

**A note on diagnostic presentation throughout this phase:** Every diagnostic, metric, and visualization produced in this phase should be presented with clear, plain-language interpretation alongside the technical output. The goal is not to simplify past the point of usefulness — scientific rigor must be maintained in the recommendations — but to ensure that every number, plot, and statistical test is accompanied by an explanation of what it means for this specific project's decisions. For example: a trace plot shouldn't just be shown; it should be accompanied by an explanation of what "good" convergence looks like, what "bad" would look like, and what specifically this trace plot tells you about whether to trust the model's output. When a metric has a threshold (e.g., R-hat should be below 1.01), explain both what the metric measures and why that threshold matters, not just whether the number passes. When tradeoffs exist between metrics (e.g., a model that's more accurate but less calibrated), present the tradeoff explicitly with a recommendation and its reasoning, not just the raw numbers. The intent is that you should be able to make sound decisions from the diagnostic output without needing to separately Google what each metric means — but the interpretation should not sand off the complexity where the complexity matters.

### 7.1 — Training Data Preparation

Construct the training dataset from your features. Critical checks:

🟢 **AUTOMATED GUARD — Leakage test:** Verify that no feature for any heat uses information from that heat or any future heat. This can be tested programmatically: for every row, confirm that all feature values can be computed from data with timestamps strictly before the heat's start time.

🟢 **AUTOMATED GUARD — Class balance check:** What's the distribution of your target variable? For heat winners in 2-person heats, it should be close to 50/50 by definition. For event winners or podiums, it will be heavily skewed (most surfers don't win/podium). Document the balance and decide if any resampling or weighting is needed.

### 7.2 — Initial Model Training

Train the model on the training set. Produce:

**Training diagnostics:** For Bayesian models: trace plots (with interpretation of what convergence vs. non-convergence looks like), convergence checks (R-hat values, explained in context), effective sample sizes (what "enough" means for your model size and complexity). For tree-based models: feature importance rankings (with discussion of whether SHAP-based importance differs from built-in importance and why), tree depth distributions. For regression: coefficient values with confidence intervals (what does the magnitude and sign of each coefficient mean substantively?), significance levels (with honest discussion of what p-values do and don't tell you in a predictive modeling context vs. a causal inference context).

**In-sample fit:** How well does the model explain the training data? Present this with context: what range of in-sample accuracy would be expected for a "good" model on this type of problem? If it's nearly perfect, that's a red flag for overfitting, not a celebration — explain why.

### 7.3 — Overfitting Detection

🟡 **AGENTIC CHECK:** Compare in-sample performance to out-of-sample performance (using the validation approach from Phase 8). A large gap suggests overfitting. Specifically monitor:

**Performance gap:** If training accuracy is 80% but validation accuracy is 55%, something is wrong. Present the gap with a benchmark: how large a gap is "normal" for this model type on this data size, and at what point does the gap become concerning?

**Feature-by-feature stability:** Remove features one at a time and retrain. If removing a feature causes a large change in performance on validation but not on training, that feature may be capturing noise. Present this as a table showing each feature's contribution and stability.

**Learning curves:** Plot performance vs. training set size. If the model keeps improving with more training data, you may need more data (or fewer features). If it plateaus early, you have enough data for the model's complexity. Annotate the plot with interpretation of what the curve's shape implies for the model's behavior.

### 7.4 — Calibration Assessment

🔴 **HUMAN GATE:** This is critical for betting. Produce a calibration plot (also called a reliability diagram): bin your predicted probabilities (e.g., 0–10%, 10–20%, ..., 90–100%) and plot the actual win rate in each bin. If the model is well-calibrated, the plot will follow the diagonal. Present this with: a clear explanation of how to read the plot, what "above the diagonal" vs. "below the diagonal" means in practical terms (the model is underconfident vs. overconfident), and how much deviation from the diagonal is acceptable. If the model consistently over- or under-predicts, apply a calibration correction (Platt scaling or isotonic regression) and re-assess — with an explanation of what each correction method does, when to prefer one over the other, and how to verify it actually improved calibration without overfitting the calibration itself.

Why this gets a human gate: miscalibration is the single most dangerous model problem for a betting system. A model that's 65% accurate but poorly calibrated will lose money. A model that's 60% accurate but well-calibrated can make money if the calibration is precise enough to identify value bets.

### Phase 7 Gate

🟡 **AGENTIC CHECK** + 🔴 **HUMAN REVIEW:**

1. Training converged properly (no warnings, divergences, or instability).
2. In-sample performance is reasonable but not suspiciously high.
3. No obvious overfitting based on train/validation gap.
4. Calibration plot is acceptable (or post-hoc calibration has been applied and verified).
5. Feature importances align with domain knowledge (sanity check — if a nonsensical feature is highly ranked, investigate).
6. All diagnostics have clear interpretations — you understand what every number and plot means and what decisions follow from them.

🔵 **RE-ANCHOR CHECK:** Re-read Phase 0 causal graph, Phase 5 feature decisions, and Phase 6 model choice rationale. Does the trained model's behavior (feature importances, key predictors) align with the causal structure you hypothesized? If the model is relying on features you didn't expect, or ignoring features you thought were important, investigate before proceeding.

---

<a name="phase-8"></a>
## Phase 8: Model Testing & Validation

**Purpose:** Rigorously evaluate the model on held-out data to get an honest assessment of how it will perform on future, unseen events.

### 8.1 — Validation Strategy

**The critical principle:** Sports data is time-ordered. Standard random train/test splits are invalid because they allow the model to "peek" at future data during training. You must use time-based splits.

**Walk-forward validation (recommended primary approach):** Train on all data up to time T, predict the next event(s), then advance T and repeat. This mimics how the model would actually be used in practice — you always train on the past and predict the future. The downside is computational cost (you retrain many times), but for a dataset this size, that's not a problem.

**Expanding window vs. sliding window:** Expanding window (always train from the start) uses more data but may be diluted by outdated patterns. Sliding window (only train on the last N events) uses less data but captures recent trends better. Test both and compare.

**Season-level holdout:** Hold out entire seasons as test sets (e.g., train on 2015–2021, test on 2022; train on 2015–2022, test on 2023). This gives you a clean assessment of how the model handles a completely new season.

### 8.2 — Target Metrics

Test the model against multiple target variables. The core three are the most important, but additional targets may offer betting value depending on what markets are available:

**Heat winner (core):** Binary — did the surfer win this heat? This is the most granular and gives you the most data points.

**Event podium — top 3 (core):** Did the surfer finish in the top 3 at the event? Fewer data points but a different bet type.

**Event winner (core):** Did the surfer win the event outright? Very few positive cases — challenging to model but potentially valuable for betting if the odds are generous.

**Additional targets to evaluate if market data supports them:**

**Quarterfinal or better:** A "make the quarters" bet is less extreme than podium and gives you more positive cases to work with. If the platform offers "top 8" or similar markets, this is worth testing.

**Highest heat score in a round / event:** Some platforms offer prop bets on scoring. If the model is good at predicting not just who wins but by how much (score margin), this opens a separate bet type.

**Head-to-head matchup props:** Even without a formal heat context, some platforms offer "Surfer A vs. Surfer B — who finishes higher at the event?" These are effectively matchup bets that don't require both surfers to be in the same heat.

**Season-level futures:** Will surfer X win the season championship? These are typically available before the season starts and at various points during it, with odds that move as the season progresses. Your model may have an edge here if it can estimate season trajectories better than the market.

**Round-specific advancement:** Will a surfer advance past a specific round? This combines heat-level modeling with bracket structure modeling.

During Phase 9 (Betting Algorithm Research), the platforms you're working with will determine which of these targets are actually bettable. But testing the model on all of them now gives you maximum flexibility later.

### 8.3 — Performance Metrics

For each target, compute:

**Accuracy:** Simple win rate of predictions. Useful but can be misleading (predicting the favorite always might get 60% accuracy but never find value bets).

**Brier score:** Measures the accuracy of probabilistic predictions. Lower is better. This is arguably the most important metric for a betting model because it penalizes both inaccuracy and overconfidence.

**Log loss:** Similar to Brier but penalizes confident wrong predictions more severely.

**Calibration metrics:** Calibration plot, expected calibration error (ECE).

**ROI-relevant metrics:** When the model says a surfer has >X% chance of winning, how often are they right? Slice this by confidence level to understand where the model is most and least reliable.

### 8.4 — Bootstrapped Confidence Intervals

Don't just report point estimates of performance. Bootstrap your test set (resample with replacement, compute metric, repeat 1000+ times) to get confidence intervals. If your Brier score is 0.22 ± 0.05, that's very different from 0.22 ± 0.01. This tells you how much of your performance might be luck.

### 8.5 — Comparison to Baseline Models

Your model's absolute performance is less meaningful than its performance relative to simple baselines:

**Always pick the higher-ranked surfer:** How often does the higher-ranked surfer actually win?

**Always pick the betting favorite (if odds data available):** How often does the market-implied favorite win? This is your real benchmark — you need to beat the market, not just beat random.

**Elo-only model:** If you built a simple Elo system alongside a more complex model, how much does the complexity actually buy you?

### 8.5b — NXTbets Blog Odds MVP Comparison (If Available)

If you extracted odds data from NXTbets blog posts (identified in Phase 1.3), this is where you use it. Take the specific surfers and events covered in those blog posts, generate predictions from the model for those same heats/events, and compare your model's implied probabilities against the NXTbets-published odds. This is a small sample and shouldn't be over-interpreted, but it gives you an early directional signal: is the model in the same universe as the betting market, or is it wildly off? If the model's probabilities diverge significantly from the market, that's either evidence of potential edge or evidence of a model problem — and you need to reason carefully about which.

### 8.6 — Error Analysis

Don't just measure performance — understand the errors. When the model gets it wrong, is there a pattern?

**Does the model fail more in certain rounds?** (e.g., good at predicting early rounds but poor at predicting finals, where presumably the surfers are more evenly matched.)

**Does it fail more at certain breaks?** (e.g., consistently wrong at a particular venue — maybe the conditions are unusual there.)

**Does it fail when an injury or form change happened that isn't in the data?** (Points to a feature gap.)

**Does it fail differently for the men's vs. women's circuit?** (If modeling both, compare error patterns across circuits.)

### Phase 8 Gate

🔴 **HUMAN GATE:** This is the most important gate in the entire project. You must answer:

1. Does the model beat the simple baselines, and by how much?
2. Does it beat the market (if odds data is available for comparison)?
3. Are the confidence intervals tight enough to be confident this isn't luck?
4. Is the model well-calibrated across the probability range?
5. Are the errors understandable and consistent with known limitations?
6. Which target metrics (heat, podium, event, others) does the model perform best on? Does this align with available betting markets?
7. **Critical question: Is this model good enough to bet real money on?** If not, what would need to improve — more data, better features, a different model, or is this problem potentially not solvable?
8. **Update the Assumptions Register:** What assumptions has the model validated? What remains uncertain?

---

<a name="phase-9"></a>
## Phase 9: Betting Algorithm Research

**Purpose:** Understand betting theory deeply enough to convert model probabilities into a profitable betting strategy. This is a research phase — no implementation yet.

### 9.1 — Core Concepts to Research

**Expected Value (EV):** The fundamental concept. If your model says a surfer has a 40% chance of winning and the odds imply 30% (i.e., the sportsbook is offering better odds than the true probability), the bet has positive expected value. Understand how to calculate EV from model probabilities and decimal/fractional/American odds.

**The Vig/Juice:** The sportsbook's built-in margin. If a fair coin flip should be even money (2.00 in decimal odds), a sportsbook might offer 1.91 on each side, keeping ~4.5% as profit. Your model edge must exceed the vig to be profitable.

**Closing Line Value (CLV):** The odds just before an event starts are the "closing line" and represent the market's best estimate of true probability. Consistently beating the closing line is the gold standard for sports bettors — and also the metric sportsbooks use to identify sharps to ban. This creates a tension: the metric that proves your model works is also the metric that gets you limited.

### 9.2 — Staking Strategies to Research

**Kelly Criterion:** Mathematically optimal bet sizing that maximizes the logarithm of wealth over time. Given your model's estimated edge, Kelly tells you what fraction of your bankroll to wager. It's theoretically optimal but in practice is extremely volatile — a bad run can wipe out a large chunk of your bankroll.

**Fractional Kelly:** Betting a fraction (commonly 1/4 to 1/2) of what full Kelly recommends. Sacrifices some expected growth for much less volatility. Most serious bettors use this.

**Fixed percentage staking:** Bet a fixed percentage of your bankroll regardless of edge size. Simpler but doesn't optimize for larger edges.

**Flat staking:** Bet a fixed dollar amount. Simplest, but doesn't grow with your bankroll or adjust for edge.

**Minimum edge threshold:** Regardless of staking strategy, set a minimum edge (model probability minus implied probability) below which you don't bet at all. This prevents placing bets where the edge is smaller than the uncertainty in your model.

### 9.3 — Bankroll Management

**Starting bankroll:** How much are you willing to allocate to this, with the understanding that you could lose all of it?

**Maximum drawdown tolerance:** At what point do you stop and re-evaluate? (e.g., "If I lose 30% of my initial bankroll, I pause and review the model.") Define this in advance — not in the moment when you're emotionally invested.

**Bet correlation:** If you bet on Surfer A to win the heat and also to win the event, those bets are correlated — if A loses the heat, both bets lose. Your staking strategy needs to account for this, or you'll be inadvertently taking larger positions than you intend.

**Ruin probability:** What's the probability of losing your entire bankroll given your bet sizing and number of bets? There are formulas for this. Compute it. If it's uncomfortably high, reduce bet sizes.

### 9.4 — Platform Strategy

**Multi-platform approach:** Spreading bets across multiple platforms reduces the risk of any one platform limiting you, and lets you shop for the best odds on each bet.

**Exchange vs. sportsbook tradeoffs:** Exchanges charge a commission on winnings but don't limit winners. Sportsbooks offer potentially better odds but will limit you if you win consistently. Understand this tradeoff and design your strategy accordingly.

**Bet timing:** Odds change over time. When should you place your bet — early (when your model might have an edge over an immature line) or late (when you have more information)? This has implications for both profitability and detection risk.

### 9.5 — Simulation Plan

Before implementing the betting algorithm, design the simulation you'll run in Phase 10. Define: what historical period will you simulate? How will you model odds availability? What metrics will you track (total P&L, ROI, max drawdown, Sharpe ratio, number of bets, win rate)? How many Monte Carlo iterations will you run?

### Phase 9 Gate

🔴 **HUMAN GATE:** Review your understanding of betting theory and confirm:

1. Do I understand Kelly criterion well enough to implement it (and its fractional variant)?
2. Have I set a maximum drawdown threshold?
3. Do I have a plan for handling correlated bets?
4. Have I thought through the platform strategy (which platforms, how to manage limitation risk)?
5. Is the simulation plan realistic given available historical odds data?
6. **Decision Log entry:** Record the chosen staking strategy, minimum edge threshold, and bankroll management rules.

---

<a name="phase-10"></a>
## Phase 10: Betting Strategy Backtesting & Simulation

**Purpose:** Test the betting strategy on historical data to estimate realistic long-term performance.

### 10.1 — Historical Backtesting

Using the model's out-of-sample predictions from Phase 8 and historical odds (if available) or simulated odds, run the full betting algorithm over the historical period.

Track per-bet: date, event, heat, surfer, model probability, implied probability from odds, edge, stake amount, outcome, profit/loss.

Track cumulative: running bankroll, running P&L, running ROI, number of bets, win rate.

🟢 **AUTOMATED GUARD:** Flag any simulation run where the bankroll goes below a defined threshold (e.g., 50% of starting) at any point — even if it recovers. This tells you about the worst-case path, not just the endpoint.

### 10.2 — Monte Carlo Simulation

Because your historical sample is limited, use Monte Carlo simulation to understand the range of possible outcomes. Resample your bet history with replacement (or simulate synthetic bet sequences based on your estimated edge and variance), and run the staking strategy thousands of times. This gives you a distribution of outcomes rather than a single backtest path.

Report: median final bankroll, 5th and 95th percentile outcomes, probability of losing money, probability of doubling bankroll, maximum drawdown distribution.

### 10.3 — Sensitivity Analysis

Vary key parameters and observe the impact:

**Edge erosion:** What happens if your actual edge is 50% of what you estimated? 25%? At what point does the strategy go from profitable to unprofitable?

**Vig variation:** What if the vig is higher than you assumed?

**Kelly fraction:** Compare full Kelly, 1/2 Kelly, 1/4 Kelly, flat staking across the same bet set.

**Minimum edge threshold:** What happens when you only bet on high-confidence picks (high edge threshold) vs. betting on anything with positive expected value (low threshold)?

### 10.4 — Multi-Year Performance Analysis

Don't just look at the aggregate. Break performance down by year, by event, by bet type (heat vs. event), and by confidence level. Look for: years where the strategy would have lost money (and understand why), bet types where the edge is concentrated, and whether performance is consistent or lumpy.

### Phase 10 Gate

🔴 **HUMAN GATE:** This is the "real money" decision gate. You must answer:

1. Is the backtested ROI positive after accounting for the vig?
2. Is the maximum drawdown within my tolerance?
3. Is the probability of ruin acceptably low (< 5% ideally)?
4. Does the Monte Carlo simulation show profitability in the majority (>60%) of scenarios?
5. Is performance reasonably consistent across years, or was it driven by a small number of outlier bets?
6. Does the sensitivity analysis show the strategy is robust to reasonable parameter variation?
7. **Am I willing to risk real money on this, with full understanding that past performance may not repeat?**

If the answers aren't convincingly positive, loop back to the appropriate phase (model improvement, feature engineering, or even Phase 0 re-evaluation).

---

<a name="phase-11"></a>
## Phase 11: Paper Trading

**Purpose:** Run the system live without real money to verify that real-world execution matches the backtest.

### 11.1 — Live Prediction Pipeline

Set up the system to generate predictions before each event/heat. Record: the prediction, what odds were available at the time, what bet the algorithm would have placed, and the outcome.

### 11.2 — Execution Gap Analysis

Compare paper trading results to what the backtest predicted. Look for:

**Odds discrepancy:** Are the odds you can actually get as good as the historical odds you backtested with? If real-time odds are worse, your backtest overestimated profitability.

**Timing issues:** By the time you see a prediction and check the odds, have they moved?

**Data latency:** Is the data you're using for live predictions as current as you assumed?

**Missing markets:** Are there heats/events where your model wants to bet but no odds are available?

### 11.3 — Duration

Paper trade for at least one full event (ideally 2–3) to get a meaningful sample. Rushing this phase to start betting real money is a classic mistake.

### Phase 11 Gate

🔴 **HUMAN GATE:**

1. Do live predictions match backtest-expected accuracy?
2. Are actual available odds close to backtest-assumed odds?
3. Is the execution pipeline reliable (no crashes, no missed predictions)?
4. Am I still confident in the system after watching it operate in real time?

---

<a name="phase-12"></a>
## Phase 12: Live Deployment — Micro Stakes

**Purpose:** Start betting real money at the minimum possible amounts to validate the full pipeline end-to-end.

### 12.1 — Minimum Viable Bets

Use the smallest bet size the platform allows. The goal isn't to make money — it's to verify that the entire chain works: prediction → bet identification → bet placement → outcome tracking → bankroll update.

### 12.2 — Bet Confirmation Layer

🔴 **HUMAN GATE on every bet (initially):** Before any bet is placed, the system presents: what bet it wants to make, why (model probability, implied probability, edge, stake), and asks for your confirmation. You can relax this to periodic review once you trust the system, but start with per-bet approval.

### 12.3 — Performance Tracking

Build a simple dashboard or report that tracks: cumulative P&L, running ROI, number of bets, win rate, average edge, and comparison to backtest expectations.

🟢 **AUTOMATED GUARD:** If live performance deviates significantly from backtest expectations (e.g., ROI is more than 2 standard deviations below expected after 30+ bets), raise an alert.

### 12.4 — Platform Health Monitoring

🟡 **AGENTIC CHECK:** Monitor for signs of platform limitation: reduced maximum bet sizes, delayed bet acceptance, odds that move against you consistently before you can bet. Document any anomalies.

### Phase 12 Gate

🔴 **HUMAN GATE:** After a defined period (e.g., 20–50 bets):

1. Is live performance within the range predicted by backtesting?
2. Have I been limited on any platform?
3. Is the execution pipeline working reliably?
4. Am I comfortable scaling up to normal bet sizes?

---

<a name="phase-13"></a>
## Phase 13: Ongoing Monitoring & Iteration

**Purpose:** Maintain the system and detect when things change.

### 13.1 — Model Drift Detection

🟢 **AUTOMATED GUARD:** Track model calibration over time (rolling Brier score, rolling calibration). If calibration degrades beyond a threshold, flag for review. This catches: surfer retirements/rookies not in training data, format changes, changes in judging tendencies.

### 13.2 — Edge Decay Monitoring

🟡 **AGENTIC CHECK:** Track your closing line value over time. If your CLV is shrinking — meaning the market is more often in agreement with your model by closing time — your edge may be eroding. This can happen because the market is getting more efficient, or because your model is stale while the world has changed.

### 13.3 — Retraining Schedule

Define when you'll retrain the model: after each season? Mid-season? Only when drift is detected? There's a tradeoff between freshness and stability — retraining too often on small new data can introduce noise.

### 13.4 — Feature & Model Re-evaluation

At least annually, revisit: are there new data sources available? Has the sport's format changed? Are there new modeling techniques worth testing? This loops back to earlier phases as needed.

🔵 **RE-ANCHOR CHECK:** At least annually, re-read this entire master plan from the beginning. Verify that the system as it currently operates still matches the plan's intentions. Document any divergences in the Decision Log.

---

<a name="dq-framework"></a>
## Cross-Cutting: Data Quality Framework

This isn't a phase — it's a system of checks that runs throughout all phases. The philosophy: **trust nothing, verify everything, and make verification visible.**

### At Data Ingestion (Phases 3–4)

🟢 **Schema validation:** Does the incoming data match the expected schema (field names, types, value ranges)?

🟢 **Completeness check:** Are there unexpected nulls? What percentage of each field is populated?

🟢 **Referential integrity:** Does every heat reference a valid event? Does every score reference a valid surfer and heat?

🟢 **Duplicate detection:** Are there duplicate records? (This is especially common with web-scraped data.)

🟡 **Temporal consistency:** Are events in chronological order? Are there impossible date combinations (e.g., a heat result recorded before the event started)?

### At Feature Engineering (Phase 5)

🟢 **Leakage test:** No feature uses information from the current or future time period.

🟢 **Range check:** All features fall within expected ranges.

🟡 **Distribution check:** Feature distributions haven't changed dramatically from the training period (distributional shift).

### At Model Input (Phases 7–8)

🟢 **Missing value check:** No unexpected nulls in the feature matrix.

🟢 **Dimensionality check:** Feature matrix has the expected number of rows and columns.

🟡 **Input distribution check:** The distribution of inputs for this prediction batch is within the range of the training data (no extreme extrapolation).

### At Model Output (Phases 7–8, 11–12)

🟢 **Probability range check:** All output probabilities are between 0 and 1.

🟢 **Sum check:** For a heat, do the probabilities of each surfer winning sum to approximately 1?

🟡 **Confidence distribution check:** Are the model's predicted probabilities distributed similarly to training outputs, or is it unusually confident/uncertain?

### At Bet Placement (Phase 12)

🟢 **Stake limit check:** No individual bet exceeds the defined maximum stake.

🟢 **Exposure check:** Total exposure across correlated bets doesn't exceed the defined limit.

🔴 **Confirmation check (initially):** Human approval before every bet.

---

<a name="checkpoints"></a>
## Cross-Cutting: Additional Guardrails & Practices

### Decision Log

Maintain a running document that records every significant decision: what was decided, why, what alternatives were considered, and what the key tradeoff was. This isn't bureaucracy — it's the thing that saves you when you need to debug a problem six months from now and can't remember why you chose a particular feature window or model type.

### Assumptions Register

A living document that lists every assumption the system depends on. Examples: "WSL scoring system hasn't changed since 2019." "Surfline forecast data is available for all CT venues." "Historical odds from OddsPortal are accurate." Periodically review and verify these haven't become false.

### Versioning Strategy

All project artifacts are managed via Git (GitHub username: `cowabungasurfbetco`). The repository should contain: all code (ETL, feature engineering, modeling, betting algorithm), configuration files, this plan document, the Phase 0 domain reference, the decision log, and the assumptions register. Data files that are too large for Git should be tracked via Git LFS or stored externally with version-stamped filenames and referenced in the repository. Trained model files should be versioned with clear naming (e.g., `model_v3_2026-03-15.pkl`) and the training data hash or version should be recorded alongside each model version. When you retrain or change anything, commit with a descriptive message. This lets you answer: "I changed X, and performance changed — was it because of X, or something else?"

### "What Could Go Wrong" Checklist

At each phase gate, explicitly ask:

1. What assumption, if wrong, would invalidate the work done in this phase?
2. What data could be silently incorrect without failing any automated check?
3. What error in this phase would compound most dangerously in later phases?
4. Is there a simpler version of what I just built that I should test first?

### Rubber Duck Agent

At any point in the project, you should be able to explain what you're doing and why to a non-technical person (or a rubber duck, or a fresh Claude conversation with no context). If you can't explain it simply, you may not understand it well enough, and that's where compounding errors hide.

---

<a name="appendix-a"></a>
## Appendix A: Key Questions to Answer Before Building Anything

These are questions that should have clear answers before you leave Phase 0. If you can't answer them, that's fine — but flag them as unknowns and plan to resolve them.

1. How many years of usable historical data exist — for both men's and women's circuits?
2. What's the smallest unit of data I can get — heat level, wave level, or event level?
3. Can I get condition data matched to specific heats, or only to events?
4. Do historical odds exist for surf, and if so, from when? If not, is the forward-looking collection plan in place?
5. Which betting platforms currently offer surf markets, and what bet types? If too limited, has the market creation path been evaluated?
6. Has anyone published a surf prediction model I can learn from (even if I don't copy it)?
7. What's the approximate vig on surf bets? (This determines the minimum edge I need.)
8. How many bets per year would my system likely place? (This determines how long it takes to reach statistical significance.)
9. What's my total budget for this project (both bankroll for betting and time investment)?
10. What would "success" look like to me — a profitable hobby, a side income, or just an interesting intellectual exercise?
11. How are surf betting odds actually generated, and what does that imply about where inefficiency might exist?
12. Is there a meaningful difference in predictability or market depth between the men's and women's circuits?

---

<a name="appendix-b"></a>
## Appendix B: Concepts to Research in Phase 9

This is a reading list for the betting algorithm research phase, not content itself. The actual research should happen in Phase 9.

**Foundational concepts:** Expected value, implied probability, odds conversion, vig/juice, closing line value, market efficiency.

**Staking theory:** Kelly criterion (original paper by John L. Kelly Jr., 1956), fractional Kelly variants, risk of ruin calculations, bet correlation handling.

**Practical betting strategy:** Value betting vs. arbitrage, the difference between beating the closing line and being profitable, why calibration matters more than accuracy for betting, how to handle the "flat spot" problem (long losing streaks even with positive expectation).

**Platform dynamics:** How sportsbooks set odds, how they adjust lines, how they identify and limit sharp bettors, exchange mechanics (Betfair-style), prediction market mechanics (Kalshi, Polymarket, Manifold), market creation processes and liquidity bootstrapping.

**Sports-specific modeling literature:** Elo systems in individual sports (tennis Elo, golf Elo), Bradley-Terry models in sports, hierarchical Bayesian models for sports prediction, the difference between modeling team sports and individual sports.

**Causal inference in sports:** Judea Pearl's DAG framework applied to sports prediction, structural causal models, potential outcomes framework (Rubin), confounding in observational sports data, interaction effects in causal models.

---

## Appendix C: Re-Anchor Check Schedule

This is a master list of all re-anchor checks in the plan, for easy reference:

| Trigger Point | What to Re-Read | What to Check For |
|---------------|-----------------|-------------------|
| Phase 0 → Phase 1 transition | Phase 0 domain reference | Completeness of domain doc |
| Phase 1 → Phase 2 transition | Phase 0 domain reference + master plan | Data sources align with causal hypotheses |
| Start of Phase 3 | Phase 0 domain reference + Phase 2 schema | Pilot exercises all schema entities |
| Phase 4 → Phase 5 transition | Phase 0 causal graph + Assumptions Register | Hypothesized variables vs. actual data |
| Start of Phase 5 | Phase 0 causal graph | Features map to causal nodes |
| Phase 6 start | Phase 0 causal graph + Phase 5 features | Model handles required causal structure |
| Phase 7 gate | Phase 0 + Phase 5 + Phase 6 decisions | Model behavior matches expectations |
| Mid-Phase 10 (random) | Full master plan | Strategy still matches plan |
| Annually during Phase 13 | Full master plan | System matches plan intentions |

Additional ad-hoc re-anchor checks should be triggered whenever something "feels off" — unexpected results, surprising feature importances, or any situation where the current work seems disconnected from the original reasoning.

---

## Final Note: The Meta-Principle

The overarching philosophy of this plan is: **slow is fast.** Every phase has gates because the cost of catching an error increases exponentially with each downstream phase that builds on it. A data error caught in Phase 3 costs you an hour. The same error caught in Phase 10 costs you weeks of rework and destroyed confidence in your results.

When in doubt, add a checkpoint. When a checkpoint seems like overkill, keep it anyway — at least for the first pass. You can always remove checkpoints later once you've built confidence in the system. You cannot retroactively add the rigor you skipped.
