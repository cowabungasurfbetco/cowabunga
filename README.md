# Cowabunga — Surf Betting Prediction System

A data-driven prediction system targeting WSL Championship Tour events. The project follows a 14-phase master plan with hard gates between phases, emphasizing causal modeling over correlational pattern-matching.

## Current Status

**Phase 0 (Domain & Viability Assessment):** Complete. Domain reference document produced. Awaiting formal gate review before Phase 1.

## Project Structure

```
cowabunga/
├── plan/                    # Master plan, domain reference, decision log
├── data/
│   ├── raw/                 # Untransformed data pulls (gitignored)
│   ├── processed/           # Cleaned and transformed (gitignored)
│   ├── odds/                # Forward-collected betting odds
│   └── samples/             # Small committed samples for reproducibility
├── notebooks/
│   ├── eda/                 # Hypothesis validation visualizations (Phase 5.0b)
│   ├── market_reconstruction/ # ALT model reverse-engineering (Phase 5.0c)
│   └── calibration/         # Model calibration analysis (Phase 7.4)
├── src/
│   ├── etl/                 # Data collection and transformation
│   ├── features/            # Feature engineering
│   ├── models/              # Model training and inference
│   ├── betting/             # Betting strategy and backtesting
│   └── monitoring/          # Drift detection, ALT reconstruction monitoring
├── tests/                   # Unit tests, leakage tests, known-answer tests
├── configs/                 # Venue metadata, model hyperparameters
├── artifacts/               # Trained model files (gitignored)
└── reports/                 # DQ reports, scenario models, calibration reports
```

## Key Documents

- [Master Plan](plan/surf_betting_master_plan_v2.md) — The canonical project plan with all phase definitions, gates, and decision points.
- [Phase 0 Domain Reference](plan/phase_0_domain_reference.md) — Competitive structure, causal hypotheses, market efficiency assessment, platform landscape.

## Principles

1. **Causal over correlational.** Every feature maps to a node in the domain causal graph.
2. **Phase gates are non-negotiable.** No phase begins until the prior gate passes.
3. **Fresh start.** All legacy work in `old/` is untrusted. Nothing is carried forward without independent re-derivation.
4. **Calibration over accuracy.** A well-calibrated 60% model beats a miscalibrated 70% model for betting.
5. **Time-awareness everywhere.** No feature ever uses future information.
