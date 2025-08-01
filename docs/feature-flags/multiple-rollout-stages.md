---
title: Measuring multiple rollout stages
sidebar_label: Multi-stage rollouts
slug: /feature-flags/multiple-rollout-stages
keywords:
  - owner:liz
last_update:
  date: 2025-06-25
---

## Continuous Analysis

For gate rules that rollout with a pass percentage ≤ 50% and without any rollback, data collected at earlier stages and later stages in the rollout will be consolidated into one analysis. This allows for a more complete and thorough analysis.

Valid continuous analysis rollouts include:

10% → 20%

10% → 20% → 50%

Invalid continuous analysis rollouts include:

10% → 5% (rollback)

10% → 70% (exceeds 50%)

When a rollout is no longer valid for continuous analysis, the new rollout step is analyzed separately from previous steps.

## Compatibility with Other Gate Features

### Balanced Gates

Gate rules that have multiple rollout stages are also [balanced](https://docs.statsig.com/feature-flags/view-exposures#balanced-gates) using downsampling.

In cases where pass percentage ≤ 50% we use the same hashing as the pass/fail and take an equal percentage of the other group.

When pass percentage > 50% we use an orthogonal random hashing to sample the larger group at a rate of $\frac{1-p}{p}$ where p is the fraction of users in the larger group. In the case of a gradual rollout, this prevents bias based on enrollment time period.

### CUPED/CURE

CUPED/CURE is available only when there have been no rollbacks AND when the rollout percentage has not exceeded 50% within a gate rule. Analysis is no longer continuous when there's a rollback or when 50% is exceeded, which means the pre-exposure data of a given unit is actually biased by earlier treatments.
