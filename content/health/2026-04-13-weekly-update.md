---
title: "Health Week: April 07 — April 13, 2026"
date: 2026-04-13
draft: false
layout: "post"
tags: ["health", "weekly-update"]
authors: ["HealthBot"]
description: "Weekly health metrics: sleep, activity, productivity, nutrition"
---

## Sleep

| Metric | This Week | Previous Week | Trend |
|--------|-----------|---------------|-------|
| Avg Bedtime | 834.0 | 714.2 | +119.8 |
| Avg Wake Time | 453.0 | 597.2 | -144.2 |
| Avg Awakenings | 7.0 | 1.2 | +5.8 |


## Activity

| Metric | This Week | Previous Week | Trend |
|--------|-----------|---------------|-------|
| Total Steps | 1,563.0 | 30,493.0 | -28,930.0 |
| Active Minutes | 476.0 min | 483.0 min | -7.0 min |
| Floors | 0.0 | 0.0 | Stable |


## Body Composition

No body composition data available this week.



### Weight Trend

{{< chart id="chart-weight-trend" type="line" height="300" >}}
{"labels": ["03-17", "03-18", "03-19", "03-22", "03-23", "03-26", "04-01", "04-05"], "datasets": [{"label": "Weight (kg)", "data": [140.2, 138.1, 139.3, 141.1, 139.0, 136.8, 135.2, 133.6], "borderColor": "#22c55e", "backgroundColor": "rgba(34,197,94,0.1)", "fill": true, "tension": 0.3}]}
{{< /chart >}}



### Glucose & Ketone Trends

{{< chart id="chart-glucose-ketone" type="line" height="300" >}}
{"labels": ["03-17", "03-18", "03-19", "03-28", "03-29", "03-30", "03-31", "04-01", "04-03", "04-04", "04-06"], "datasets": [{"label": "Glucose (mmol/L)", "data": [6.0, 5.4, 6.1, 6.1, 5.3, null, 5.0, null, null, null, null], "borderColor": "#f59e0b", "spanGaps": true, "tension": 0.3}, {"label": "Ketones (mmol/L)", "data": [null, null, null, 1.6, 1.0, 1.0, null, 1.1, 2.4, 2.0, 2.7], "borderColor": "#8b5cf6", "spanGaps": true, "tension": 0.3}]}
{{< /chart >}}



## Nutrition

| Day | Meals | Protein | Carbs | Fat | Calories |
|-----|-------|---------|-------|-----|----------|
| 2026-04-07 | 2 | ~58.0g | ~86.0g | ~110.0g | ~1586.0 kcal |
| 2026-04-08 | 1 | ~35.0g | ~21.0g | ~77.0g | ~936.0 kcal |
| 2026-04-10 | 2 | ~36.0g | ~31.0g | ~77.0g | ~1076.0 kcal |
| 2026-04-11 | 2 | ~92.0g | ~34.0g | ~114.0g | ~1547.0 kcal |
| **Weekly Avg** | | **~55.2g** | **~43.0g** | **~94.5g** | **~1286.0 kcal** |

7 meals logged this week.


### Daily Macro Breakdown

{{< chart id="chart-macro-breakdown" type="bar" height="300" >}}
{"labels": ["03-16", "03-17", "03-18", "03-19", "03-20", "03-21", "03-23", "03-24", "03-25", "03-26", "03-27", "03-28", "03-29", "03-30", "03-31", "04-01", "04-02", "04-03", "04-04", "04-06", "04-07", "04-08", "04-10", "04-11"], "datasets": [{"label": "Protein (g)", "data": [35.0, 34.6, 67.0, 37.7, 144.3, 47.0, 80.2, 26.2, 40.0, 95.0, 61.2, 45.9, 57.5, 57.5, 7.0, 121.9, 13.6, 90.9, 32.1, 129.0, 58.0, 35.0, 36.0, 92.0], "backgroundColor": "#ef4444"}, {"label": "Carbs (g)", "data": [21.0, 13.7, 3.0, 3.2, 38.4, 4.0, 24.1, 61.6, 2.0, 3.0, 37.6, 90.8, 2.5, 2.5, 29.0, 62.5, 15.0, 24.9, 10.2, 24.0, 86.0, 21.0, 31.0, 34.0], "backgroundColor": "#f59e0b"}, {"label": "Fat (g)", "data": [77.0, 20.0, 46.0, 43.7, 208.5, 55.0, 60.3, 23.0, 40.0, 95.0, 89.2, 47.6, 55.0, 55.0, 7.0, 123.0, 12.2, 135.3, 11.7, 131.0, 110.0, 77.0, 77.0, 114.0], "backgroundColor": "#3b82f6"}]}
{{< /chart >}}



## Productivity

| Metric | This Week | Previous Week | Trend |
|--------|-----------|---------------|-------|
| Productive Time | 5,856.0 min | 5,769.0 min | +87.0 min |
| Neutral Time | 11.0 min | 3.0 min | +8.0 min |



## Interesting Correlations

- eating_well positively correlates with steps_3000 (5 stars)

- sleep positively correlates with rem_sleep (5 stars)

- good_code positively correlates with eating_ok (5 stars)

- good_code positively correlates with steps_3000 (5 stars)

- light_sleep positively correlates with rem_sleep (5 stars)


## Weekly Reflection

Solid week for fat-adapted eating — Avocado Coffee anchoring most days with one solid meal. Protein dipped a bit on single-meal days. No new weight or glucose readings logged this week, so charts carry forward from last week. Cron automation was broken but now fixed — next week should be fully automated.
