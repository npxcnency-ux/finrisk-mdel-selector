# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a **Claude Code skill** (not a traditional code project). It provides ML algorithm selection guidance for financial risk control scenarios — expense anomaly detection, fraud detection, and compliance auditing. The skill triggers when users describe scenarios involving labeled/unlabeled data, class imbalance, explainability requirements, or financial risk modeling.

## Structure

- `SKILL.md` — Main skill definition with Step 0-4 selection framework and three-layer production architecture
- `references/algorithm-matrix.md` — Algorithm recommendation matrix, data readiness impact, imbalance handling strategies, evaluation metric selection
- `references/pitfalls.md` — Detailed gotchas: label bias, concept drift, time leakage, cold start, operational feasibility, data readiness, multicollinearity, high-cardinality encoding
- `examples/sample-recommendation.md` — Complete output example showing all 7 recommendation components
- `evals/evals.json` — 5 evaluation scenarios with assertion-based grading criteria (includes negative assertions)
- `evals/docs/lessons.md` — Skill authoring best practices (from Anthropic)

## Key Design Decisions

- **Five-step selection flow (Step 0-4)**: Business objectives → Data readiness → Label status → Volume + Imbalance (joint) → Explainability. Business objectives gate all decisions; data readiness determines model complexity ceiling before considering algorithms.
- **Business objectives first (Step 0)**: Moved from last to first — misalignment on business goals (false positive vs false negative tolerance, operational capacity) invalidates all downstream choices.
- **Data readiness assessment (Step 1)**: New step — feature count < 5 or missing rate > 50% means rule engine only. Prevents wasting effort on models that can't outperform simple rules.
- **Volume + Imbalance merged (Step 3)**: Effective anomaly sample count (total × anomaly rate) is the real decision variable, not volume or rate alone.
- **Core counterintuitive rule**: When anomaly rate < 1%, even with labels, do NOT go straight to supervised learning. Use Isolation Forest for recall expansion first, then supervised model for precision ranking.
- **Two audiences for explainability**: Ranking for compliance teams (performance-first) vs. explaining to flagged employees (strong explainability). These require different models sharing the same feature engineering.
- **Output format is prescriptive**: Every recommendation must include 7 components — business objective confirmation, data readiness assessment, algorithm + rationale, imbalance handling, explainability approach, evaluation metrics, and next steps.

## Editing Guidelines

- When modifying `SKILL.md`, preserve the Gotchas section — it captures the highest-signal failure modes that Claude defaults to without this skill.
- When adding new algorithms to `references/algorithm-matrix.md`, maintain the condition → algorithm → rationale table structure.
- Eval assertions in `evals/evals.json` test for specific reasoning steps (e.g., "identifies imbalance", "confirms business objectives"), not just algorithm names. New evals should follow this pattern.
- The skill description in `SKILL.md` frontmatter is a trigger specification, not a summary. Keep it focused on when to activate.
