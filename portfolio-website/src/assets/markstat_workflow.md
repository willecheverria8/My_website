---
title: "Is Settler Mortality a Valid Instrument? Identification, Inference, and Sensitivity in the AJR Framework (2001, 2012)"
author: "William Enrique Echeverría Tigse"
date: "January 2026"
output:
  html:
    toc: true
    toc-depth: 3
    number-sections: true
---

> **Generated output:** Do not edit this HTML directly. Edit `markstat_workflow.stmd` and re-run `markstat`.
>  
> **Primary script:** `markstat_workflow.do` (called by Markstat).  
> **Data:** AJR replication package (AER/ICPSR), see Replication Guide below.

<style>
.key-results {
  border: 1px solid #d0d7de;
  border-radius: 8px;
  padding: 12px 16px;
  background: #f6f8fa;
}
.key-results h3 { margin-top: 0; }
.star1 { color: #2e7d32; font-weight: 600; }
.star2 { color: #388e3c; font-weight: 600; }
.star3 { color: #1b5e20; font-weight: 700; }
.meta { text-align: center; }
.abstract-centered { text-align: center; }
.code-map {
  border-left: 4px solid #d0d7de;
  padding: 8px 14px;
  background: #fbfbfb;
  border-radius: 6px;
}
</style>

# Abstract

<div class="abstract-centered">
This paper evaluates the validity of the settler mortality instrument proposed by Acemoglu, Johnson, and Robinson (2001), as claimed in their 2012 response to Albouy. It treats the debate as a case study in the identification and inference of instrumental variables. Using the AJR (2012) replication dataset, I reproduce the baseline first- and two-stage least squares estimates and examine how inference changes with instrument construction, variance–covariance assumptions, clustering schemes, and weak-instrument–robust procedures (including Anderson–Rubin confidence sets). The results suggest that point estimates of institutional effects are relatively stable, but statistical inference is sensitive to identification and inference choices. The workflow is fully reproducible and integrates Stata code blocks with each table and figure to support graduate-level teaching of IV econometrics.
</div>

# Replication Guide

<div class="code-map">

**Data source (AJR 2012 replication package):**  
- AER download: `https://www.aeaweb.org/aer/data/oct2012/20110390_data.zip`  
- ICPSR citation / DOI: `https://doi.org/10.3886/E112564V1`

**Expected folder structure (relative to project root):**
- `data/` (raw `.dta` files placed here)
- `assets/tables/` (LaTeX and HTML tables exported here)
- `assets/figures/` or `graphs/` (saved figures)
- `markstat_workflow.stmd` (this file)
- `markstat_workflow.do` (master do-file)

**Build (example):**
1. Put the AJR `.dta` in `data/`
2. Run: `markstat using markstat_workflow.stmd, strict`  
   (or run `markstat_workflow.do` if you prefer a direct do-file pipeline)

</div>

# 1. Introduction

Acemoglu, Johnson, and Robinson (2001) ask why income per capita differs so sharply across countries and emphasize institutions—particularly property-rights institutions—as a determinant of incentives and long-run development. The empirical obstacle is endogeneity: institutions may shape development, but development may also reshape institutions, and both may reflect omitted historical and geographic factors. 

AJR propose historical settler mortality as a source of plausibly exogenous variation: mortality conditions affected settlement feasibility; settlement strategies shaped colonial institutions; and institutional persistence links those choices to contemporary outcomes. This paper uses the AJR (2012) replication materials to evaluate the identification and inference properties of that IV framework. The focus is econometric—relevance, weak identification, and sensitivity to variance–covariance assumptions—while broader historical debates are discussed elsewhere.

<div class="key-results">
### Key Results Snapshot (placeholders — update once final tables are locked)

| Result | Magnitude | Interpretation |
|--------|-----------|----------------|
| **2SLS effect (baseline)** | ~0.9–1.1 log points | 1-unit ↑ in institutional quality → large ↑ in income |
| **First-stage strength** | varies by controls | clustered F falls sharply in saturated models |
| **Weak-IV inference** | AR sets widen / may be unbounded | inference depends on identification strength |
</div>

# 2. Identification Framework

## 2.1 Structural relationship

We interpret the empirical problem as estimating the causal effect of institutional quality on economic performance:

$$
\ln(GDP_{pc,i}) = \beta_0 + \beta_1 \, Risk_i + X_i'\beta_2 + u_i
$$

where $Risk_i$ is protection against expropriation (institutional quality) and $X_i$ includes geography and continent indicators.

## 2.2 Why OLS is not causal

OLS is inconsistent when $\text{Cov}(Risk_i, u_i) \neq 0$, due to reverse causality, omitted confounders, and measurement error.

## 2.3 Identification assumptions (2SLS)

We instrument $Risk_i$ with settler mortality $Z_i=\log(Mortality_i)$ and rely on:
- **Relevance:** $\pi_1 \neq 0$ in the first stage.
- **Exclusion restriction:** conditional on $X_i$, settler mortality affects income only through institutions.
- **Monotonicity (LATE):** higher mortality does not increase inclusive-institution formation for any unit.

## 2.4 Measurement controversy and instrument credibility

Albouy (2012) argues mortality coding is heterogeneous across colonies and may embed measurement error and comparability problems. AJR (2012) respond with alternative mortality series and robustness checks. This paper treats the exchange as an identification and inference stress test rather than a binary verdict.

## 2.5 Inference under weak identification

Even when first-stage F-statistics appear adequate, inference can be unreliable in finite samples or under clustered dependence and grouped instruments. The analysis therefore reports weak-instrument–robust Anderson–Rubin confidence sets alongside conventional inference.

# 3. Data and Descriptive Statistics

This section documents the AJR (2001) dataset as updated in AJR (2012), describes the core variables, and reports summary statistics by mortality quartiles and region.

## 3.1 Data and variables

{{data_load_and_labels}}

## 3.2 Summary statistics

{{summary_stats_table}}

## 3.3 Descriptive plots

{{descriptive_plots}}

# 4. Baseline Estimates

This section reports (i) OLS benchmarks, (ii) first-stage evidence, (iii) 2SLS estimates, and (iv) reduced-form estimates. Specifications mirror AJR’s baseline structure.

## 4.1 OLS benchmarks

{{ols_table_and_plot}}

## 4.2 First-stage results and instrument relevance

{{first_stage_table_and_plot}}

> **Note:** Interpret signs carefully. If higher `risk` means *more protection* (as in the PRS scale), then a negative coefficient on `logmort0` implies higher mortality → weaker institutions (lower protection). If `risk` is coded inversely, adjust wording accordingly.

## 4.3 Second-stage (2SLS) results and weak-IV robust inference

{{second_stage_table_with_ar}}

## 4.4 Reduced-form evidence

Reduced-form regressions estimate:
$$
\ln(GDP_{pc,i}) = \delta_0 + \delta_1 \log(Mortality_i) + X_i'\delta_2 + \varepsilon_i
$$
They summarize the *total* association between mortality and income that the IV strategy attributes to the institutional channel.

{{reduced_form_table_and_plot}}

# 5. Robustness and Sensitivity Analysis

We assess sensitivity to (i) alternative mortality measures, (ii) endogeneity tests, (iii) weak identification diagnostics, and (iv) alternative variance–covariance assumptions and clustering schemes.

## 5.1 Alternative settler mortality measures

{{table7_ajr_robustness}}

## 5.2 Endogeneity of institutional quality (DWH-type tests)

{{endogeneity_tests}}

## 5.3 Weak identification and robust inference

{{weak_iv_diagnostics}}

## 5.4 Alternative clustering schemes

{{vce_sensitivity_table}}

# 6. Lessons for Applied Instrumental Variables

This section extracts teaching-oriented takeaways on diagnosis, weak-IV inference, and the interaction between instrument construction and standard error assumptions.

{{lessons_section}}

# 7. Conclusions and Future Research

We summarize replication results and outline extensions that connect the AJR framework to elite governance and distributional structures (e.g., treating inequality as a proxy for elite configuration and studying its causal effects on welfare outcomes).

{{conclusion_future_research}}

---

# References

- Acemoglu, D., Johnson, S., & Robinson, J. A. (2001). The colonial origins of comparative development: An empirical investigation. *American Economic Review, 91*(5), 1369–1401.  
- Acemoglu, D., Johnson, S., & Robinson, J. A. (2012). The colonial origins of comparative development: An empirical investigation: Reply. *American Economic Review, 102*(6), 3077–3110.  
- Albouy, D. Y. (2012). The colonial origins of comparative development: An empirical investigation: Comment. *American Economic Review, 102*(6), 3059–3076.  
- Anderson, T. W., & Rubin, H. (1949). Estimation of the parameters of a single equation in a complete system of stochastic equations. *Annals of Mathematical Statistics, 20*(1), 46–63.  
- Staiger, D., & Stock, J. H. (1997). Instrumental variables regression with weak instruments. *Econometrica, 65*(3), 557–586.  
- Stock, J. H., & Yogo, M. (2005). Testing for weak instruments in linear IV regression. In D. W. K. Andrews & J. H. Stock (Eds.), *Identification and inference for econometric models* (pp. 80–108). Cambridge University Press.  
- Kleibergen, F., & Paap, R. (2006). Generalized reduced rank tests using singular value decomposition. *Journal of Econometrics, 133*(1), 97–126.
