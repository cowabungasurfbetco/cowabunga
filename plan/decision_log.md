# Decision Log

All significant project decisions are recorded here with reasoning at the time of the decision. When something goes wrong later, this log lets you trace back to where the chain broke.

---

## D-001: Separate Men's and Women's Models
**Date:** Phase 0
**Decision:** Build separate models for men's and women's CT circuits.
**Reasoning:** Structural differences (field size: 36 vs 18 pre-2026, aerial frequency gap, historical condition inequality, scoring pattern differences) are large enough that a single model would either compromise on both or overfit to the larger men's dataset. Separate models allow circuit-specific feature engineering and calibration.

## D-002: 2019+ as Primary Training Set
**Date:** Phase 0
**Decision:** Use 2019+ data as the primary training set. Pre-2019 data is secondary (used for Elo initialization and long-term surfer skill estimation only).
**Reasoning:** The 2019 shift from 3-person to 2-person heats is a major structural break. A model trained on 3-person heat dynamics will learn patterns that don't apply to the current format. Pre-2019 data is still valuable for estimating surfer career trajectories and initializing Elo ratings, but shouldn't drive the primary prediction model.

## D-003: Hybrid DAG + Rubin Causal Framework
**Date:** Phase 0
**Decision:** Use Pearl DAGs for causal structure specification and Rubin potential outcomes for estimation where appropriate.
**Reasoning:** DAGs force explicit specification of causal direction and confounders. Rubin's framework provides the estimation machinery (particularly for treatment-effect-style questions like "what is the causal effect of priority on win probability"). The hybrid leverages the strengths of both.

## D-004: Target Outright Winner and H2H Bets
**Date:** Phase 0
**Decision:** Primary bet types are outright event winner and heat-level head-to-head.
**Reasoning:** H2H bets are the most direct application of the model (binary prediction maps cleanly). Outright winner bets offer larger payoffs and may be less efficiently priced due to the combinatorial complexity of predicting a full bracket.

## D-005: Sportsbooks Primary, Prediction Markets Contingency
**Date:** Phase 0
**Decision:** Target existing sportsbooks as the primary betting channel. Prediction market creation (Kalshi) is a contingency/long-term path.
**Reasoning:** Sportsbooks offer immediate access and existing liquidity. Prediction markets require market creation, liquidity bootstrapping, and regulatory navigation — viable long-term but slower to execute. The model is the same regardless of channel.

## D-006: Preliminary 97% Model Accuracy is Unreliable
**Date:** Phase 0
**Decision:** Discard the old model's 97% accuracy claim entirely. Do not use it as evidence of edge or as a baseline.
**Reasoning:** 97% accuracy on surf heat prediction is not credible. Almost certainly the result of data leakage (future information in training features) or improper cross-validation (standard k-fold on time-series data). The fresh start directive applies.
