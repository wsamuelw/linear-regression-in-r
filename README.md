# Linear Regression in R

Predicting sales from advertising spend — simple vs multiple linear regression, from manual `lm()` to tidymodels workflow.

## Problem

How much does each marketing channel contribute to sales? This project builds two models on the same dataset: a simple linear regression (total spend → sales) and a multiple linear regression (youtube, facebook, newspaper → sales) to show how adding features changes model performance.

## What's Inside

| Script | Approach | Features | R² |
|--------|----------|----------|-----|
| `simple linear regression.R` | Base R `lm()` | `all_channels` (sum of all spend) | 0.753 |
| `multiple linear regression.R` | tidymodels `linear_reg()` | youtube, facebook, newspaper separately | 0.901 |

## Results

### Simple Linear Regression (Total Spend → Sales)

| Metric | Value |
|--------|-------|
| R² | 0.753 |
| RMSE | 3.10 |
| MAE | 2.34 |
| Intercept | 5.09 |
| Coefficient | 0.049 |

75% of sales variability is explained by total marketing spend alone.

```
predicted_sales = 5.09 + 0.049 × total_spend
```

- $0 spend → predicted sales = $5.09 (baseline)
- $300 spend → predicted sales = $19.70

### Multiple Linear Regression (Per-Channel → Sales)

| Metric | Value |
|--------|-------|
| R² (train) | 0.901 |
| R² (test) | 0.925 |
| RMSE (test) | 1.74 |
| Intercept | 3.31 |

| Channel | Coefficient | p-value | Interpretation |
|---------|-------------|---------|---------------|
| YouTube | 0.046 | < 2e-16 | Strongest predictor — every $1 on YouTube → $0.046 in sales |
| Facebook | 0.190 | 1.38e-44 | Highest per-dollar return — every $1 on Facebook → $0.19 in sales |
| Newspaper | 0.005 | 0.418 | Not statistically significant |

Adding channels as separate features boosts R² from 0.75 → 0.90. The key insight: **Facebook has the highest per-dollar return** (0.19 vs 0.046), but YouTube's coefficient is also significant because it has higher absolute spend in the data.

## Key Findings

- **Newspaper advertising has no measurable effect** — p-value of 0.418, not significant at any reasonable threshold
- **Facebook is 4x more effective per dollar** than YouTube (0.19 vs 0.046)
- **Total spend explains 75%** of sales — channel breakdown pushes it to 90%
- **RMSE drops from 3.10 to 1.74** when separating channels — 44% improvement

## Setup

```bash
git clone https://github.com/wsamuelw/linear-regression-in-r.git
cd linear-regression-in-r
```

```r
install.packages(c("tidyverse", "tidymodels", "Metrics", "broom", "vip"))
source("simple linear regression.R")
```

## Data

**Marketing** — from `datarium` package. 200 observations of advertising spend across three channels and resulting sales.

| Feature | Min | Median | Max | Unit |
|---------|-----|--------|-----|------|
| youtube | 0.84 | 179.70 | 355.68 | $ spent |
| facebook | 0.00 | 27.48 | 59.52 | $ spent |
| newspaper | 0.36 | 30.90 | 136.80 | $ spent |
| sales | 1.92 | 15.48 | 32.40 | units sold |

## Linear Regression in 30 Seconds

**Simple**: one feature → one coefficient

```
y = β₀ + β₁x
```

**Multiple**: many features → many coefficients

```
y = β₀ + β₁x₁ + β₂x₂ + β₃x₃
```

**Evaluation**:
- **R²** — % of variance explained (higher = better, max 1.0)
- **RMSE** — average prediction error in original units (lower = better)
- **p-value** — is this feature statistically significant? (p < 0.05 = yes)

## Tech Stack

- **base R** — `lm()` for simple regression
- **tidymodels** — `linear_reg()` for multiple regression
- **broom** — tidy model summaries
- **vip** — feature importance plots
- **Metrics** — RMSE, MSE, MAE calculations
- **datarium** — marketing dataset

## References

- [Evaluating regression models](https://towardsdatascience.com/what-are-the-best-metrics-to-evaluate-your-regression-model-418ca481755b)
- [datarium marketing data](https://rpkgs.datarium/reference/marketing.html)

## License

MIT
