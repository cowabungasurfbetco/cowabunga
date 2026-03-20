# Video Analysis Scoping Document

**Status:** Deferred — not part of the core plan. To be evaluated as a potential Phase 5b sub-project if structured data features prove insufficient for edge.

**Last updated:** 2026-03-20

---

## Why Video Data Matters

Competition video contains information that structured datasets don't capture: how a surfer moves, what maneuvers they attempt, how they select waves, how they manage priority, and how their performance changes within a heat. These are potentially high-value features precisely because they're labor-intensive to extract — meaning the market (ALT Sports Data, generic oddsmakers) is unlikely to incorporate them, making them candidates for genuine edge.

---

## Video-Derivable Feature Candidates

### Wave Selection & Strategy
- **Wave selection patterns:** Does a surfer tend to pick off smaller, safer waves or wait for bigger, riskier ones?
- **Positioning and priority strategy:** How does a surfer use the priority system tactically?
- **Time between waves:** Could indicate fatigue, strategic patience, or wave quality assessment.
- **Wave count efficiency:** Ratio of scoring waves to total waves attempted.

### Physical Indicators
- **Paddling strength/fitness:** How quickly a surfer paddles into waves — a proxy for physical condition.
- **Fatigue indicators:** Do scores or attempt rates decline within a heat? Does paddling speed decrease?

### Style Classification
- **Riding style:** Cautious vs. aggressive, aerial-focused vs. power-focused, signature moves.
- **Risk appetite:** Frequency of high-variance scoring attempts (big turns, airs) vs. conservative surfing.

### Aerial Risk/Reward Analysis (Primary Sub-Workstream)

Aerial maneuvers deserve dedicated attention due to a hypothesized asymmetric risk/reward profile: higher scores when completed, but lower completion rates than traditional maneuvers.

**Data to capture:**
- **Per-surfer aerial propensity:** How often does each surfer attempt airs in competition? This is a proxy for style and risk appetite.
- **Per-surfer aerial completion rate:** A surfer who attempts airs frequently but lands only 30% is a different risk profile than one who attempts rarely but lands 70%.
- **Condition-dependent aerial viability:** Airs may be more feasible in certain wave types (open-face, lighter winds) and less in others (heavy barrels, strong onshore). The interaction between surfer aerial propensity and condition suitability could be a powerful predictive feature.
- **Women's vs. men's aerial gap:** Investigate whether aerial frequency differs significantly between circuits, and whether any gap is converging over time.
- **Temporal trends:** Are aerials becoming more common and higher-scoring over time? If so, surfers who adopt aerial strategies earlier may gain a compounding advantage.

---

## Scoping Approach

### Minimum Viable Sample
Start with 50–100 heats across a range of surfers and conditions. For each heat, classify each wave as aerial attempt or traditional maneuver, record completion (landed/fell), and note the score awarded. Build per-surfer aerial profiles from this sample.

### Video Source Assessment
- Determine what footage is available (WSL archives, YouTube highlights, broadcast recordings).
- Assess cost of access (see Phase 0.4c cost landscape assessment).
- Determine whether timestamp markers exist for each wave within a heat's video recording — this would dramatically reduce review time by pinpointing exactly when each wave occurs.

### Effort Estimate
- Manual classification: ~15–20 minutes per heat for a trained reviewer (identify each wave, classify maneuver type, record completion and score).
- For 100 heats: ~25–35 hours of manual work.
- Semi-automated approach (if wave timestamps exist): ~5–10 minutes per heat, reducing to ~8–15 hours for 100 heats.

### Decision Criteria for Activation
This workstream should be activated only if:
1. Phase 5 feature engineering reveals that structured data features alone are insufficient — either model accuracy plateaus or the ALT reconstruction (Phase 5.0c) shows the market already prices the structured features.
2. The video-derivable features identified above map to unpriceable factors on the edge map.
3. The effort estimate (25–35 hours for an MVP sample) is proportionate to the expected edge improvement.

---

## Integration with Core Plan

If activated, video-derived features would enter the pipeline at Phase 5 (Feature Engineering) as additional candidate features. They would go through the same quality checks (5.2), sanity checks (5.3), and importance analysis (5.5) as any other feature. The key question at the Phase 5 gate would be: do video-derived features add predictive power beyond what structured data features already provide?

---

## What This Document Is NOT

This is not a plan to build a computer vision pipeline. The approach is manual or semi-automated human review, not ML-based video analysis. Computer vision for surf maneuver classification is a research problem in its own right and is out of scope for this project.
