# Metrics guide

Every metric describes the **measured responses in the current filter scope**: the prompts, platforms, regions, and completed Scans selected. None of them estimates every answer an AI assistant could give to every potential customer.

## The five headline metrics

| Metric             | Definition                                                     | Scale             | Direction        |
| ------------------ | -------------------------------------------------------------- | ----------------- | ---------------- |
| **Visibility**     | Responses mentioning the brand ÷ eligible responses × 100      | 0–100 %           | Higher is better |
| **Share of Voice** | Mentions of the brand ÷ mentions of all tracked brands × 100   | 0–100 %           | Higher is better |
| **Sentiment**      | Average tone of the statements AI made about the brand         | 0–100, 50 neutral | Higher is better |
| **Gap**            | 100 − Visibility: responses that did **not** mention the brand | 0–100 %           | Lower is better  |
| **Health**         | Rounded average of Visibility, Share of Voice, and Sentiment   | 0–100             | Higher is better |

## What each metric cannot tell you

- **Visibility** counts appearances only. It says nothing about whether the mention was prominent, favourable, accurate, or persuasive — check Sentiment and the raw responses for that.
- **Share of Voice** depends on which competitors are tracked. Adding or removing a tracked competitor changes the comparison group, so values from either side of that change are not comparable.
- **Sentiment** is an average of classified statements, not a percentage of positive responses. A score of 62 does not mean 62% of responses were positive. Every score is backed by quoted statements; read them before explaining a surprising value, especially when few statements contributed. Scans finalised before the claims cutover used a −100 to 100 scale — flag the scale if comparing across that boundary.
- **Gap** is missed brand presence, not the distance to the strongest competitor. Do not present it as a competitor-performance gap.
- **Health** blends a sentiment score with two percentages. Never present it as market share, customer percentage, or likelihood of being recommended, and always name which component drives a good or bad Health value. When Sentiment is missing, Health averages the remaining two components.

## Changes between Scans

A delta compares the current result with the previous completed Scan in scope, in **points**, not percent: a move from 40 to 46 is +6 points, not a 6% increase. Prompt, platform, region, competitor, and filter changes all affect the comparison — a movement shows _that_ the measurement changed, not _why_.

## Missing data semantics

- **null / dash**: not enough data in the current selection. This is missing evidence, not a bad score.
- **Empty filtered result**: no measured responses match the filters. Not zero.
- **Zero**: a calculated result — 0% Visibility means the brand appeared in none of the eligible responses.
- A value built on a handful of responses can move sharply between Scans. Read the underlying responses before calling such a movement a trend.
