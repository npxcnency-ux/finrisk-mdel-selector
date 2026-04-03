# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a **Claude Code skill** (not a traditional code project). It provides ML algorithm selection guidance for financial risk control scenarios — expense anomaly detection, fraud detection, and compliance auditing. The skill triggers when users describe scenarios involving labeled/unlabeled data, class imbalance, explainability requirements, or financial risk modeling.

## Structure

- `SKILL.md` — Main skill definition with five-step selection framework and three-layer production architecture
- `references/algorithm-matrix.md` — Algorithm recommendation matrix, imbalance handling strategies, evaluation metric selection
- `references/pitfalls.md` — Detailed gotchas: label bias, concept drift, time leakage, cold start, operational feasibility, multicollinearity, high-cardinality encoding
- `examples/sample-recommendation.md` — Complete output example showing all 5 recommendation components
- `evals/evals.json` — 5 evaluation scenarios with assertion-based grading criteria (includes negative assertions)
- `evals/docs/lessons.md` — Skill authoring best practices (from Anthropic)

## Key Design Decisions

- **Five-step selection flow**: Label status → Imbalance level → Data volume → Explainability requirement → Business value tradeoff. This order is deliberate — label status gates everything else, data volume determines which algorithms are viable before considering explainability.
- **Core counterintuitive rule**: When anomaly rate < 1%, even with labels, do NOT go straight to supervised learning. Use Isolation Forest for recall expansion first, then XGBoost for precision ranking.
- **Two audiences for explainability**: Ranking for compliance teams (performance-first) vs. explaining to flagged employees (strong explainability). These require different models sharing the same feature engineering.
- **Output format is prescriptive**: Every recommendation must include 5 components — algorithm + rationale, imbalance handling, explainability approach, evaluation metrics, and next steps.

## Editing Guidelines

- When modifying `SKILL.md`, preserve the Gotchas section — it captures the highest-signal failure modes that Claude defaults to without this skill.
- When adding new algorithms to `references/algorithm-matrix.md`, maintain the condition → algorithm → rationale table structure.
- Eval assertions in `evals/evals.json` test for specific reasoning steps (e.g., "identifies imbalance"), not just algorithm names. New evals should follow this pattern.
- The skill description in `SKILL.md` frontmatter is a trigger specification, not a summary. Keep it focused on when to activate.
