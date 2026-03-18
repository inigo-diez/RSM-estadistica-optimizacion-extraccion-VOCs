# RSM — Aromatic Extraction Optimisation

> **Final Degree Project** · Complete statistical experimental analysis with Central Composite Design (CCD) and 9 response variables.

---

## Table of contents

1. [Project description](#1-project-description)
2. [Experimental design](#2-experimental-design)
3. [System variables](#3-system-variables)
4. [Notebook structure](#4-notebook-structure)
5. [Workflow](#workflow)
6. [Figures and tables — detailed explanation](#5-figures-and-tables--detailed-explanation)
   - 5.1 [CCD classification](#51-ccd-classification)
   - 5.2 [Normality verification and transformations](#52-normality-verification-and-transformations)
   - 5.3 [Main effects plots](#53-main-effects-plots)
   - 5.4 [Interaction plots](#54-interaction-plots)
   - 5.5 [Correlations and preliminary residuals](#55-correlations-and-preliminary-residuals)
   - 5.6 [Preliminary models (Phase 3)](#56-preliminary-models-phase-3)
   - 5.7 [Full quadratic RSM fit (Phase 4)](#57-full-quadratic-rsm-fit-phase-4)
   - 5.8 [In-depth analysis of Y₅ — Indole](#58-in-depth-analysis-of-y--indole)
   - 5.9 [Effect interpretation: Pareto, Surfaces, Pred vs Obs (Phase 5)](#59-effect-interpretation-pareto-surfaces-pred-vs-obs-phase-5)
   - 5.10 [Optimum localisation — Hessian analysis (Phase 6)](#510-optimum-localisation--hessian-analysis-phase-6)
   - 5.11 [Multi-response optimisation — desirability (Phase 7)](#511-multi-response-optimisation--desirability-phase-7)
   - 5.12 [Per-experiment desirability with expanded bounds](#512-per-experiment-desirability-with-expanded-bounds)
7. [Final conclusions](#6-final-conclusions)
8. [Repository files](#7-repository-files)
9. [Requirements](#8-requirements)
10. [Bibliographic references](#9-bibliographic-references)

---

## 1. Project description

This work applies **Response Surface Methodology (RSM)** to simultaneously model and optimise nine response variables characterising the aromatic quality of an extraction process. The central objective is to identify the operating conditions — extraction time, desorption time and incubation time — that maximise the recovery of target compounds (Indole, Cresol, Methional and carboxylic acids) while minimising method variability.

The analysis is structured in seven progressive phases: from the verification of distributional assumptions through to global optimisation via the weighted desirability function of Derringer and Suich (1980), encompassing full quadratic fitting, analytical localisation of the optimum via Hessian analysis, and robustness evaluation through sensitivity analysis.

---

## 2. Experimental design

A **Central Composite Design (CCD)** with three factors and 17 experiments was used, distributed in three blocks:

| Point type | No. experiments | Coded values |
|---|---|---|
| Factorial | 8 | xᵢ = ±1 |
| Axial (star) | 6 | xᵢ = ±α (α ≈ 1.68) |
| Central (replicates) | 3 | xᵢ = 0 |

The central replicates (experiments 14, 15 and 16) allow estimation of **Pure Error** with 2 degrees of freedom, enabling the Lack-of-Fit test independently of the residual error.

### Factor coding

| Real factor | Unit | Centre (C) | Step (S) | Coded variable |
|---|---|---|---|---|
| Extraction time | min | 75 | 45 | x₁ = (t − 75) / 45 |
| Desorption time | min | 2 | 1 | x₂ = (t − 2) / 1 |
| Incubation time | min | 10 | 5 | x₃ = (t − 10) / 5 |

Coding orthogonalises the factors, eliminates correlation between linear and quadratic terms, and facilitates direct comparison of coefficients as a measure of each factor's relative effect.

---

## 3. System variables

### Factors (independent variables)

| Symbol | Description | Real range |
|---|---|---|
| x₁ | Extraction time | 30 – 120 min |
| x₂ | Desorption time | 1 – 3 min |
| x₃ | Incubation time | 5 – 15 min |

### Responses (dependent variables)

| Symbol | Description | Objective |
|---|---|---|
| Y₁ | No. of target metabolites detected | Maximise |
| Y₂ | Total no. of metabolites detected | Maximise |
| Y₃ | %RSD over all metabolites | Minimise |
| Y₄ | %RSD over target metabolites | Minimise |
| Y₅ | Relative fraction Indole/All | Maximise |
| Y₆ | Relative fraction Cresol/All | Maximise |
| Y₇ | Relative fraction Methional/All | Maximise |
| Y₈ | Relative fraction Carboxylic acids/All | Maximise |
| Y₉ | Fraction Targets/Total | Maximise |

---

## 4. Notebook structure

```
RSM_Optimization_VOCs.ipynb
│
├── Section 1  — CCD classification and normality verification
├── Section 2  — Main effects and interactions
├── Section 3  — Correlations and preliminary residuals
├── Section 4  — Preliminary models (linear and with interactions)
├── Section 5  — Full quadratic RSM fit (Phases 4.1–4.6)
├── Section 6  — In-depth analysis of Y₅ (Indole)
├── Phase 5    — Pareto of effects · 3D Surfaces · Pred vs Obs
├── Phase 6    — Critical point and Hessian diagnosis
├── Phase 7    — Multi-response optimisation (desirability)
└── Correction — Per-experiment desirability with expanded bounds
```

---

## Workflow

```
┌─────────────────────────────────────────────────────────────┐
│                       INPUT DATA                            │
│  Variables_y.xlsx · datos_transformados.xlsx                │
│  Diseño Experimental.xlsx  (17 exp., 3 factors, 9 resp.)    │
└────────────────────────┬────────────────────────────────────┘
                         │
                         ▼
┌─────────────────────────────────────────────────────────────┐
│  STEP 1 — Assumption verification                           │
│  · Shapiro-Wilk test per variable                           │
│  · Transformations √Y / log(Y) if p < 0.05                 │
│  · Factor orthogonality (|r| < 0.15)                        │
└────────────────────────┬────────────────────────────────────┘
                         │
                         ▼
┌─────────────────────────────────────────────────────────────┐
│  STEP 2 — Visual exploration                                │
│  · Main Effects Plots (9 variables × 3 factors)             │
│  · Interaction Plots  (9 variables × 3 pairs)               │
│  · Residuals and collinearity of linear models              │
└────────────────────────┬────────────────────────────────────┘
                         │
                         ▼
┌─────────────────────────────────────────────────────────────┐
│  STEP 3 — Preliminary models                                │
│  · Pure linear: Y = β₀ + β₁x₁ + β₂x₂ + β₃x₃               │
│  · With interactions: + β₁₂x₁x₂ + β₁₃x₁x₃ + β₂₃x₂x₃      │
│  · R² comparison and significant terms                      │
└────────────────────────┬────────────────────────────────────┘
                         │
                         ▼
┌─────────────────────────────────────────────────────────────┐
│  STEP 4 — Full quadratic RSM fit (central model)            │
│  · OLS on 10-parameter model (statsmodels)                  │
│  · ANOVA: F_reg, F_LoF, Pure Error (centre replicates)      │
│  · Coefficient table with p-values                          │
│  · Metrics: R², R²_adj, RMSE, PRESS                         │
└────────────────────────┬────────────────────────────────────┘
                         │
                         ▼
┌─────────────────────────────────────────────────────────────┐
│  STEP 5 — Effect interpretation                             │
│  · Pareto of |t| per variable (significant effects)         │
│  · 3D surfaces + contour maps (3 planes × 9 responses)      │
│  · Predicted vs Observed + visual RMSE                      │
└────────────────────────┬────────────────────────────────────┘
                         │
                         ▼
┌─────────────────────────────────────────────────────────────┐
│  STEP 6 — Individual optimum localisation                   │
│  · Critical point: ∂Ŷ/∂xᵢ = 0  →  x* = −½ B⁻¹ b           │
│  · Hessian analysis: eigenvalues → max / min / saddle       │
└────────────────────────┬────────────────────────────────────┘
                         │
                         ▼
┌─────────────────────────────────────────────────────────────┐
│  STEP 7 — Multi-response optimisation                       │
│  · Individual desirability dᵢ ∈ [0,1] (Derringer & Suich)  │
│  · Global desirability D = weighted geometric mean          │
│  · Global search: differential evolution + L-BFGS-B         │
│  · Sensitivity analysis: ±15 % perturbation in L/T/U/w     │
└────────────────────────┬────────────────────────────────────┘
                         │
                         ▼
┌─────────────────────────────────────────────────────────────┐
│  METHODOLOGICAL CORRECTION                                  │
│  · L = μ − 2σ  (instead of L = observed minimum)           │
│  · D_global for each of the 17 observed experiments         │
│  · Identification of the optimal observed experiment        │
└─────────────────────────────────────────────────────────────┘
                         │
                         ▼
┌─────────────────────────────────────────────────────────────┐
│                       OUTPUTS                               │
│  Graficos/  (63 PNG figures)                                │
│  RSM_Completo_Todos_Y.xlsx                                  │
│  Resumen_Comparativo_RSM.xlsx                               │
│  Resultados_RSM_Fases567.xlsx                               │
└─────────────────────────────────────────────────────────────┘
```

---

## 5. Figures and tables — detailed explanation

### 5.1 CCD classification

**Internal notebook table** classifying the 17 experiments into their three categories (factorial, axial, central) with the coded values of x₁, x₂ and x₃. This classification is the starting point of the analysis: it confirms that the design meets the requirements of a valid CCD — approximate orthogonality, estimability of all quadratic terms, and existence of replicates for the Lack-of-Fit test.

The fact that x₃ takes the value ±0.82 at factorial points (instead of the theoretical ±1) indicates that the design was built with α = 1.68 under the **rotatability** criterion (α = 2^(k/4)), which guarantees that prediction variance is constant at equidistant points from the centre.

---

### 5.2 Normality verification and transformations

**Figure:** 9×2 grid of histograms (original vs transformed distribution) with fitted normal density curves.

![Normality — histograms original vs transformed](Graficos/normalidad_histogramas.png)

**Table:** Normality summary with Shapiro-Wilk p-values, boolean normality indicator (p > 0.05) and transformation applied.

The **Shapiro-Wilk** test (Shapiro & Wilk, 1965) is the most appropriate for small samples (n = 17). When the normality hypothesis is rejected, Box-Cox family transformations are evaluated (Box & Cox, 1964): square root √Y for count data and logarithmic log(Y) for data with variance proportional to the mean. The transformation choice is based on the maximum log-likelihood criterion of the λ parameter.

Residual normality is an OLS model assumption underpinning RSM; its verification on the original variable serves as a prior diagnostic before the formal fit.

---

### 5.3 Main effects plots

**Figure:** 9 individual panels (one per response variable). Each panel contains 3 boxplots — one per factor — with experimental points superimposed. The horizontal axis distinguishes the low (−1), central (0) and high (+1) level of each coded factor.

| Y₁ | Y₂ | Y₃ |
|:--:|:--:|:--:|
| ![Effects Y1](Graficos/efectos_Y1_MetabDiana.png) | ![Effects Y2](Graficos/efectos_Y2_MetabTotales.png) | ![Effects Y3](Graficos/efectos_Y3_RSD_Todos.png) |

| Y₄ | Y₅ | Y₆ |
|:--:|:--:|:--:|
| ![Effects Y4](Graficos/efectos_Y4_RSD_Diana.png) | ![Effects Y5](Graficos/efectos_Y5_Indol.png) | ![Effects Y6](Graficos/efectos_Y6_Cresol.png) |

| Y₇ | Y₈ | Y₉ |
|:--:|:--:|:--:|
| ![Effects Y7](Graficos/efectos_Y7_Metional.png) | ![Effects Y8](Graficos/efectos_Y8_AcCarboxilicos.png) | ![Effects Y9](Graficos/efectos_Y9_DianasTotales.png) |

**Table:** Most impactful factor per variable, calculated as the factor maximising |Δ| = |mean(high level) − mean(low level)|.

Main Effects Plots allow a first qualitative reading of each factor's effect in isolation, with other factors averaged. They are particularly useful for detecting non-linear effects: if the response at the central level lies above (or below) the line connecting the extremes, there is evidence of curvature, which in the quadratic RSM model is captured by coefficients β₁₁, β₂₂, β₃₃.

---

### 5.4 Interaction plots

**Figure:** 9 × 3 panels (one row per response variable, one column per factor pair: x₁×x₂, x₁×x₃, x₂×x₃). Each panel shows the mean response at low and high level of one factor, differentiated by the level of the crossed factor.

| Y₁ | Y₂ | Y₃ |
|:--:|:--:|:--:|
| ![Interaction Y1](Graficos/interaccion_Y1_MetabDiana.png) | ![Interaction Y2](Graficos/interaccion_Y2_MetabTotales.png) | ![Interaction Y3](Graficos/interaccion_Y3_RSD_Todos.png) |

| Y₄ | Y₅ | Y₆ |
|:--:|:--:|:--:|
| ![Interaction Y4](Graficos/interaccion_Y4_RSD_Diana.png) | ![Interaction Y5](Graficos/interaccion_Y5_Indol.png) | ![Interaction Y6](Graficos/interaccion_Y6_Cresol.png) |

| Y₇ | Y₈ | Y₉ |
|:--:|:--:|:--:|
| ![Interaction Y7](Graficos/interaccion_Y7_Metional.png) | ![Interaction Y8](Graficos/interaccion_Y8_AcCarboxilicos.png) | ![Interaction Y9](Graficos/interaccion_Y9_DianasTotales.png) |

**Table:** Qualitative classification of each interaction (strong / moderate / weak / none) with overall interpretation per variable.

When the two lines of the interaction plot are parallel, the interaction is null: the effect of one factor is independent of the other's level. When lines cross, the interaction is strong and individual effects cannot be interpreted in isolation. This visual exploration anticipates which of the crossed terms β₁₂x₁x₂, β₁₃x₁x₃ and β₂₃x₂x₃ will be significant in the quadratic model.

---

### 5.5 Correlations and preliminary residuals

**Figure 1 — Factor correlation heatmap:** Shows Pearson correlation between x₁, x₂ and x₃. In a well-constructed CCD the coded factors are approximately orthogonal (|r| < 0.05), guaranteeing independent estimability of each coefficient.

![Correlation heatmap — between factors](Graficos/correlacion_heatmap.png)

![Y vs Y correlations](Graficos/correlacion_YvsY.png)

**Figure 2 — Preliminary linear models + residual histograms:** For each Y a pure linear model (without quadratics or interactions) is fitted and the distribution of its residuals evaluated. A symmetric histogram centred at zero suggests the linear model captures the central trend and residuals show no structure.

![Residual histograms — preliminary linear models](Graficos/residuos_histogramas.png)

**Figure 3 — Residuals vs fitted scatter:** Detects heteroscedasticity (non-constant variance) and structure in residuals that the linear model does not capture. A funnel or U-shaped pattern indicates unmodelled curvature, reinforcing the need for the quadratic RSM model.

![Residuals vs fitted — linear models](Graficos/residuos_vs_predichos.png)

**Table:** Collinearity summary between factors with categorical interpretation.

---

### 5.6 Preliminary models (Phase 3)

**Figure — Linear vs interactions comparison per variable:** For each Y, the coefficient tables of the pure linear model (Y = β₀ + β₁x₁ + β₂x₂ + β₃x₃) and the model with first-order interactions (adding β₁₂x₁x₂ + β₁₃x₁x₃ + β₂₃x₂x₃) are shown side by side, with t-values, p-values and confidence bands.

| Y₁ | Y₂ | Y₃ |
|:--:|:--:|:--:|
| ![Models Y1](Graficos/fase3_modelos_Y1_MetabDiana.png) | ![Models Y2](Graficos/fase3_modelos_Y2_MetabTotales.png) | ![Models Y3](Graficos/fase3_modelos_Y3_RSD_Todos.png) |

| Y₄ | Y₅ | Y₆ |
|:--:|:--:|:--:|
| ![Models Y4](Graficos/fase3_modelos_Y4_RSD_Diana.png) | ![Models Y5](Graficos/fase3_modelos_Y5_Indol.png) | ![Models Y6](Graficos/fase3_modelos_Y6_Cresol.png) |

| Y₇ | Y₈ | Y₉ |
|:--:|:--:|:--:|
| ![Models Y7](Graficos/fase3_modelos_Y7_Metional.png) | ![Models Y8](Graficos/fase3_modelos_Y8_AcCarboxilicos.png) | ![Models Y9](Graficos/fase3_modelos_Y9_DianasTotales.png) |

**Comparative table:** R² and adjusted R² of both models for the 9 responses, together with significant terms (p < 0.05) in each model.

The purpose of this phase is twofold: (i) identify which factors have a clear linear effect before the full quadratic fit, and (ii) quantify the increase in explained variance provided by interactions over the pure linear model, as an indicator of system complexity. These models are exploratory, **not** the definitive RSM model.

---

### 5.7 Full quadratic RSM fit (Phase 4)

This is the central phase of the analysis. The fitted model is:

```
Ŷ = β₀ + β₁x₁ + β₂x₂ + β₃x₃
      + β₁₁x₁² + β₂₂x₂² + β₃₃x₃²
      + β₁₂x₁x₂ + β₁₃x₁x₃ + β₂₃x₂x₃
```

with **n = 17 experiments**, **p = 10 parameters** and **7 residual degrees of freedom**.

#### Figure — `Comparacion_RSM.png`

![RSM comparison — R², F_reg, F_LoF](Graficos/Comparacion_RSM.png)

Three-panel horizontal bar chart comparing the 9 responses:
- **R²**: proportion of total variance explained by the quadratic model.
- **F_reg**: F-statistic of the full regression (tests H₀: all βᵢ = 0 except β₀).
- **F_LoF**: F-statistic of the Lack-of-Fit test (tests whether the quadratic model is sufficient or higher-order curvature exists).

High R² (> 0.90) together with significant F_reg (p < 0.05) and non-significant F_LoF (p > 0.05) are the three simultaneous criteria for a good RSM model. F_LoF uses the central replicates to separate pure error from model error, making its interpretation more demanding than the global F.

#### ANOVA tables per response

For each Y the complete ANOVA table is printed with the following sources of variation:

| Source | df | Sum of squares | Mean square | F | p-value |
|---|---|---|---|---|---|
| Regression | 9 | SS_reg | MS_reg | F_reg | p_reg |
| Total residual | 7 | SS_res | MS_res | — | — |
| Lack of fit | 5 | SS_LoF | MS_LoF | F_LoF | p_LoF |
| Pure error | 2 | SS_PE | MS_PE | — | — |
| Total | 16 | SS_tot | — | — | — |

#### Coefficient tables per response

For each Y the table of 10 coefficients is presented with estimate, standard error, t-statistic and two-sided p-value. Quadratic coefficients (β₁₁, β₂₂, β₃₃) indicate surface curvature; negative values imply the surface is concave downward (a maximum exists) and positive values imply it is convex (a minimum exists) in that factor's direction.

#### Master table — `RSM_Completo_Todos_Y.xlsx`

Excel export with one sheet per response containing: ANOVA table, coefficients with significances, global metrics (R², RMSE, PRESS) and predicted vs observed values for the 17 experiments.

---

### 5.8 In-depth analysis of Y₅ — Indole

Indole (Y₅) receives differentiated treatment as the only target metabolite whose relative fraction is governed by a **significant synergistic interaction x₁×x₂** (extraction time × desorption time, p = 0.016): the effect of a long extraction time only materialises in a real Indole gain when desorption time is also high.

#### Figure — `Y5_Indol_Superficies.png`

![Y5 Indole — 3D surfaces and contours](Graficos/Y5_Indol_Superficies.png)

3D response surfaces + contour maps for the three cut planes (x₁×x₂, x₁×x₃ and x₂×x₃). The third factor is fixed at its optimal value for each cut. Contour maps (Ŷ isolines) allow direct reading of the factor combinations producing the same yield and delimit the recommended operating region.

#### Figure — `Y5_Indol_Interaccion.png`

![Y5 Indole — x₁×x₂ interaction plot](Graficos/Y5_Indol_Interaccion.png)

Specific x₁×x₂ interaction plot for Y₅. The crossing lines reveal that at low desorption level (x₂ = −1) extraction time has almost no effect on the Indole fraction, whereas at high desorption level (x₂ = +1) longer extraction time produces a notable increase. This "disordinal" interaction pattern justifies searching for the optimum in the high-extraction + high-desorption corner.

---

### 5.9 Effect interpretation: Pareto, Surfaces, Pred vs Obs (Phase 5)

#### Figures — `Y1_Pareto.png` … `Y9_Pareto.png`

| Y₁ | Y₂ | Y₃ |
|:--:|:--:|:--:|
| ![Pareto Y1](Graficos/Y1_Pareto.png) | ![Pareto Y2](Graficos/Y2_Pareto.png) | ![Pareto Y3](Graficos/Y3_Pareto.png) |

| Y₄ | Y₅ | Y₆ |
|:--:|:--:|:--:|
| ![Pareto Y4](Graficos/Y4_Pareto.png) | ![Pareto Y5](Graficos/Y5_Pareto.png) | ![Pareto Y6](Graficos/Y6_Pareto.png) |

| Y₇ | Y₈ | Y₉ |
|:--:|:--:|:--:|
| ![Pareto Y7](Graficos/Y7_Pareto.png) | ![Pareto Y8](Graficos/Y8_Pareto.png) | ![Pareto Y9](Graficos/Y9_Pareto.png) |

Pareto charts of effects for each response: horizontal bars ordered from largest to smallest by the absolute t-statistic of each coefficient. The vertical line marks the critical t threshold (α = 0.05, two-sided). Terms whose bar exceeds the line are statistically significant.

The Pareto of effects immediately communicates which factors dominate each response and whether control comes from linear, quadratic or interaction effects. It allows prioritisation of factors in the optimisation stage and model simplification if desired.

#### Figures — `Y1_Superficies.png` … `Y9_Superficies.png`

| Y₁ | Y₂ | Y₃ |
|:--:|:--:|:--:|
| ![Surfaces Y1](Graficos/Y1_Superficies.png) | ![Surfaces Y2](Graficos/Y2_Superficies.png) | ![Surfaces Y3](Graficos/Y3_Superficies.png) |

| Y₄ | Y₅ | Y₆ |
|:--:|:--:|:--:|
| ![Surfaces Y4](Graficos/Y4_Superficies.png) | ![Surfaces Y5](Graficos/Y5_Superficies.png) | ![Surfaces Y6](Graficos/Y6_Superficies.png) |

| Y₇ | Y₈ | Y₉ |
|:--:|:--:|:--:|
| ![Surfaces Y7](Graficos/Y7_Superficies.png) | ![Surfaces Y8](Graficos/Y8_Superficies.png) | ![Surfaces Y9](Graficos/Y9_Superficies.png) |

For each response: 2×3 panel with 3D surfaces (top row) and contour maps (bottom row) for the three planes x₁×x₂, x₁×x₃ and x₂×x₃. The absent factor is fixed at the estimated critical point (or at the centre if the critical point lies outside the experimental region).

3D surfaces visualise the geometry of the predicted response function and allow visual identification of whether an interior maximum/minimum exists or whether the response is monotonic in the studied range (saddle or ramp shape). Contour maps are more useful for operational decision-making as they allow working regions to be established on the two-factor plane.

> **Note on scale:** all response values shown are in **real scale** (back-transformed). SQRT-transformed responses (Y₁, Y₂, Y₃, Y₅, Y₈, Y₉) are converted back as Ŷ² and LOG responses (Y₄, Y₆, Y₇) as exp(Ŷ). Fraction variables Y₅–Y₉ are multiplied by 100 to express them as percentages (%). The number of decimal places on isoline labels and the maximum marker is adjusted automatically based on the magnitude of each response (0 decimals if max ≥ 10 000, up to 5 if max < 1), ensuring readability at any scale.

#### Figure — `PredVsObs_Todas.png`

![Predicted vs Observed — 9 responses](Graficos/PredVsObs_Todas.png)

3×3 panel with Predicted vs Observed scatter plots for the 9 responses **in real scale** (back-transformed). The identity line and ±RMSE band are included. Points close to the diagonal confirm the quadratic model fits the data well. Points far from the diagonal are influential experiments or outliers deserving review.

Alongside the figure a **metrics table** is generated: R², RMSE and PRESS (Predicted Residual Error Sum of Squares, cross-validation prediction error estimator).

---

### 5.10 Optimum localisation — Hessian analysis (Phase 6)

#### Figure — `Fase6_Hessianos.png`

![Phase 6 — Critical points and Hessian analysis](Graficos/Fase6_Hessianos.png)

For each response: panel with the analytical critical point (solution of ∂Ŷ/∂xᵢ = 0, i = 1, 2, 3), the eigenvalues of the **Hessian matrix** of the quadratic model, and the critical point classification. The predicted value Y* is shown in **real scale** (back-transformed: SQRT→Ŷ², LOG→exp(Ŷ); ×100 for fraction variables).

The nature of the critical point is determined by the sign of the eigenvalues of H = ∂²Ŷ/∂xᵢ∂xⱼ:

| Condition | Classification |
|---|---|
| All eigenvalues < 0 | Maximum |
| All eigenvalues > 0 | Minimum |
| Mixed-sign eigenvalues | Saddle point |

Hessian analysis is the classical analytical approach in RSM (Myers, Montgomery & Anderson-Cook, 2016). Its limitations are: (i) the critical point may lie outside the experimental region, making the prediction an unreliable extrapolation; (ii) for response maxima, the Hessian point optimises a single response at a time, without considering the trade-off between all 9 variables simultaneously.

---

### 5.11 Multi-response optimisation — desirability (Phase 7)

#### Individual desirability function

For each response Yᵢ an individual function dᵢ ∈ [0, 1] is defined following Derringer & Suich (1980):

- **Maximise:** dᵢ = [(Yᵢ − L) / (T − L)]^s if L ≤ Yᵢ ≤ T; dᵢ = 1 if Yᵢ ≥ T; dᵢ = 0 if Yᵢ ≤ L
- **Minimise:** dᵢ = [(U − Yᵢ) / (U − T)]^s if T ≤ Yᵢ ≤ U; dᵢ = 1 if Yᵢ ≤ T; dᵢ = 0 if Yᵢ ≥ U

where L is the lower acceptable limit, T the target (desired optimum), U the upper acceptable limit and s the shape exponent (s = 1 implies linear function).

#### Weighted global desirability function

```
D_global = exp [ Σᵢ wᵢ · ln(dᵢ) / Σᵢ wᵢ ]
```

This is the **weighted geometric mean** of individual desirabilities, proposed by Derringer & Suich (1980) and widely used in analytical chemistry and process design. The weights used are:

| Response | Weight (w) | Justification |
|---|---|---|
| Y₁ (Target metabolites) | 0.20 | Primary analytical success criterion |
| Y₂ (Total metabolites) | 0.15 | Overall method coverage |
| Y₃ (%RSD all) | 0.10 | Overall precision |
| Y₄ (%RSD target) | 0.10 | Precision over target metabolites |
| Y₅ (Indole) | 0.15 | Target volatile aromatic of interest |
| Y₆ (Cresol) | 0.10 | Target aromatic of interest |
| Y₇ (Methional) | 0.10 | Target aromatic of interest |
| Y₈ (Carboxylic acids) | 0.05 | Complementary class |
| Y₉ (Targets/Total) | 0.05 | Overall selectivity |

#### Figure — `Fase7_Deseabilidades.png`

![Phase 7 — Individual desirabilities at the optimum](Graficos/Fase7_Deseabilidades.png)

Panel with the 9 dᵢ(Yᵢ) functions at the global optimum: bars showing the individual desirability value reached by each response at the optimal conditions. Allows visualisation of which responses are furthest from their target and which already reach dᵢ ≈ 1.

#### Figure — `Fase7_Optimo_Global.png`

![Phase 7 — Global multi-response optimum localisation](Graficos/Fase7_Optimo_Global.png)

Visualisation of the global optimum found by numerical optimisation (differential evolution + L-BFGS-B, Storn & Price 1997; Byrd et al. 1995) in the coded factor space. The projection of the optimum onto the three cut planes (x₁×x₂, x₁×x₃, x₂×x₃) is shown superimposed on the D_global contour map.

The optimum is located using `scipy.optimize.differential_evolution` as a stochastic global search (avoiding local minima) followed by L-BFGS-B local refinement for precision. The search runs with 50 independent random starts to guarantee convergence to the global optimum.

#### Figure — `Fase7_Sensibilidad.png`

![Phase 7 — Optimum sensitivity analysis](Graficos/Fase7_Sensibilidad.png)

Sensitivity analysis of the optimum: the L, T and U parameters of each response are perturbed by ±15 % of their range, and weights wᵢ are also perturbed, evaluating how D_global and optimum coordinates change. Bars show the relative variation of D_global under each perturbation.

A robust optimum is one whose D_global varies little (< 5 %) under reasonable specification perturbations. If any perturbation produces a large change, that specific parameter is critical and must be established with particular care in practice.

#### Table — `Resultados_RSM_Fases567.xlsx`

Excel export with three sheets: (1) Pareto of effects for the 9 responses, (2) Critical points and Hessian eigenvalues, (3) Multi-response optimum with D_global, optimal coordinates, predicted values and individual desirabilities.

---

### 5.12 Per-experiment desirability with expanded bounds

This section addresses a **relevant methodological issue** detected in the standard desirability calculation: when L = observed minimum (as is common in automatic implementations), any experiment at that minimum deterministically obtains dᵢ = 0, collapsing D_global to zero through the multiplicative effect of the geometric mean, regardless of the performance of the other responses.

**Applied solution (Expanded bounds):**
- For responses to maximise: L = mean − 2·σ
- For responses to minimise: U = mean + 2·σ

This shifts the "zero desirability floor" outside the real experimental range, so no observed experiment receives dᵢ = 0 simply for being the worst in the sample.

#### Figure — `Desirabilidad_por_Experimento.png`

![Per-experiment desirability — original vs expanded bounds](Graficos/Desirabilidad_por_Experimento.png)

Top panel: comparative bars of D_global for the 17 experiments calculated with original bounds (L = observed minimum) vs expanded bounds (L = mean − 2σ). The winning experiment with expanded bounds is the one recommended for experimental validation.

Bottom panel: breakdown of the 9 individual desirabilities dᵢ for the experiment with the highest D_global, allowing identification of which responses are furthest from their target at that point.

#### Table — `df_dc` (printed in notebook)

Table of 17 rows with real conditions (t_ext, t_des, t_inc), the 9 corrected individual desirabilities and the corrected D_global per experiment. This table is the direct decision tool: the researcher can select the optimal experiment or a trade-off experiment balancing aromatic performance and operational practicality.

---

## 6. Final conclusions

### 6.1 RSM fit quality

The quadratic RSM model offers a satisfactory fit for most responses (R² > 0.85 for Y₁, Y₂, Y₅), indicating that the CCD design space adequately covers the relevant variation region of the system. Precision-related responses (%RSD, Y₃ and Y₄) show greater residual variability, consistent with the inherently noisy nature of variation coefficients in chromatographic analysis.

### 6.2 Dominant effects

- **x₁ (extraction time)** is the most influential factor on the number of metabolites detected (Y₁, Y₂), with a predominantly positive linear effect: longer extraction times increase metabolomic coverage.
- **x₂ (desorption time)** critically controls the relative fraction of target volatile compounds (Y₅, Y₆, Y₇). Its effect on Indole and Cresol is markedly non-linear.
- **x₃ (incubation time)** exerts a secondary effect on overall extraction efficiency, modulating mainly selectivity (Y₉).
- The **x₁×x₂ interaction** is the most relevant in the system: Indole (Y₅) only reaches high fractions when extraction is long *and* desorption is intense, indicating a sequential release mechanism requiring both conditions simultaneously.

### 6.3 Recommended operating conditions

Prioritising recovery of **Indole + Cresol + Methional** (Y₅, Y₆, Y₇) via weighted multi-response optimisation (combined weights: 0.35), the global optimum is located in the region of:

- **x₁ > 0** (extraction times above the centre, ~ 90–120 min)
- **x₂ > 0** (desorption times above the centre, ~ 2.5–3 min)
- **x₃ ≈ 0** (incubation time around the central value, ~ 10 min)

This combination reflects that long extraction and energetic desorption are the drivers of aromatic volatile performance, while incubation plays a secondary role and can be adjusted for operational convenience.

### 6.4 Optimum robustness

The sensitivity analysis shows that D_global is relatively stable against ±15 % perturbations in specification parameters, indicating that the optimum is not an artefact of the specific choice of L, T and U but a real characteristic of the process response surface.

### 6.5 Observation on Y₆ and Y₇

During correlation analysis, a **perfect correlation r = 1.000** was detected between Cresol (Y₆) and Methional (Y₇), meaning both variables contain exactly the same experimental information in this dataset. It is recommended to review the origin of this redundancy (possible spreadsheet error or quantification protocol issue) before interpreting their results individually in future publications.

---

## 7. Repository files

```
RSM TFG/
│
├── RSM_Optimization_VOCs.ipynb   ← Main notebook (complete analysis)
├── Variables_y.xlsx              ← Original responses Y₁–Y₉ (17 exp.)
├── datos_transformados.xlsx      ← Transformed responses for RSM
├── Diseño Experimental.xlsx      ← Real factors (t_ext, t_des, t_inc)
│
└── Graficos/
    ├── Comparacion_RSM.png       ← R², F_reg, F_LoF comparison
    ├── PredVsObs_Todas.png       ← Predicted vs Observed (9 responses)
    ├── Fase6_Hessianos.png       ← Critical points and eigenvalues
    ├── Fase7_Deseabilidades.png  ← Individual desirabilities at optimum
    ├── Fase7_Optimo_Global.png   ← Multi-response optimum localisation
    ├── Fase7_Sensibilidad.png    ← Optimum sensitivity analysis
    ├── Desirabilidad_por_Experimento.png ← D_global per experiment
    ├── Y[1-9]_Pareto.png         ← Pareto of effects per response
    ├── Y[1-9]_Superficies.png    ← 3D surfaces + contours per response
    ├── efectos_Y[1-9]_*.png      ← Main effects plots
    ├── interaccion_Y[1-9]_*.png  ← Interaction plots
    ├── fase3_modelos_Y[1-9]_*.png← Preliminary model coefficients
    ├── normalidad_histogramas.png ← Normality histograms
    ├── correlacion_*.png          ← Correlation heatmaps
    ├── residuos_*.png             ← Residual diagnostics
    ├── RSM_Completo_Todos_Y.xlsx ← Master ANOVA + coefficients + metrics
    ├── Resumen_Comparativo_RSM.xlsx  ← Comparative table R², F, LoF
    └── Resultados_RSM_Fases567.xlsx  ← Pareto, Hessian, Multi-resp. optimum
```

---

## 8. Requirements

```bash
pip install numpy pandas matplotlib scipy statsmodels openpyxl scikit-learn seaborn
```

Recommended: **Python 3.10+** and **NumPy ≥ 1.24**. The notebook uses `%matplotlib inline` for Jupyter rendering.

To execute the full notebook:

```bash
jupyter nbconvert --to notebook --execute RSM_Optimization_VOCs.ipynb \
    --output RSM_Optimization_VOCs.ipynb --ExecutePreprocessor.timeout=480
```

---

## 9. Bibliographic references

**Experimental design and RSM:**

- Box, G. E. P., & Behnken, D. W. (1960). Some new three level designs for the study of quantitative variables. *Technometrics*, 2(4), 455–475. https://doi.org/10.1080/00401706.1960.10489912
- Box, G. E. P., & Cox, D. R. (1964). An analysis of transformations. *Journal of the Royal Statistical Society, Series B*, 26(2), 211–243. https://doi.org/10.1111/j.2517-6161.1964.tb00553.x
- Box, G. E. P., & Wilson, K. B. (1951). On the experimental attainment of optimum conditions. *Journal of the Royal Statistical Society, Series B*, 13(1), 1–45.
- Myers, R. H., Montgomery, D. C., & Anderson-Cook, C. M. (2016). *Response Surface Methodology: Process and Product Optimization Using Designed Experiments* (4th ed.). Wiley.
- Montgomery, D. C. (2017). *Design and Analysis of Experiments* (9th ed.). Wiley.

**Multi-response optimisation:**

- Derringer, G., & Suich, R. (1980). Simultaneous optimization of several response variables. *Journal of Quality Technology*, 12(4), 214–219. https://doi.org/10.1080/00224065.1980.11980968
- Harrington, E. C. (1965). The desirability function. *Industrial Quality Control*, 21(10), 494–498.

**Statistical tests:**

- Shapiro, S. S., & Wilk, M. B. (1965). An analysis of variance test for normality (complete samples). *Biometrika*, 52(3–4), 591–611. https://doi.org/10.2307/2333709
- Cook, R. D. (1977). Detection of influential observation in linear regression. *Technometrics*, 19(1), 15–18. https://doi.org/10.1080/00401706.1977.10489493

**Numerical optimisation:**

- Storn, R., & Price, K. (1997). Differential evolution — a simple and efficient heuristic for global optimization over continuous spaces. *Journal of Global Optimization*, 11(4), 341–359. https://doi.org/10.1023/A:1008202821328
- Byrd, R. H., Lu, P., Nocedal, J., & Zhu, C. (1995). A limited memory algorithm for bound constrained optimization. *SIAM Journal on Scientific Computing*, 16(5), 1190–1208. https://doi.org/10.1137/0916069

**Software:**

- Harris, C. R., et al. (2020). Array programming with NumPy. *Nature*, 585, 357–362. https://doi.org/10.1038/s41586-020-2649-2
- Seabold, S., & Perktold, J. (2010). Statsmodels: Econometric and statistical modeling with Python. *Proceedings of the 9th Python in Science Conference*, 57–61.
- Virtanen, P., et al. (2020). SciPy 1.0: Fundamental algorithms for scientific computing in Python. *Nature Methods*, 17, 261–272. https://doi.org/10.1038/s41592-019-0686-2
- Hunter, J. D. (2007). Matplotlib: A 2D graphics environment. *Computing in Science & Engineering*, 9(3), 90–95. https://doi.org/10.1109/MCSE.2007.55

---

*Analysis performed with Python 3.x · Jupyter Notebook · statsmodels · scipy · numpy · pandas · matplotlib*
