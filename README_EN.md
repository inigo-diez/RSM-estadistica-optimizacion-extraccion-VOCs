# RSM — Aromatic Extraction Optimisation

> Final Degree Project · Complete analysis using Response Surface Methodology (RSM) with a Central Composite Design (CCD)

---

## Description

Multi-response optimisation of an aromatic extraction process using RSM and a Central Composite Design (CCD, 17 exp., 3 factors). Includes full quadratic model fitting, Hessian analysis, 3D response surfaces, and weighted desirability function (Derringer & Suich, 1980).

---

## Experimental structure

| Element | Detail |
|---|---|
| Design | Rotatable Central Composite Design (CCD) |
| Experiments | 17 (8 factorial + 6 axial + 3 centre replicates) |
| Factors | x₁: extraction time (30–120 min), x₂: desorption time (1–3 min), x₃: incubation time (5–15 min) |
| Responses | Y₁–Y₉ (target metabolites, total metabolites, %RSD, Indole, Cresol, Methional, Carboxylic acids, Targets/Total) |
| Model | Full quadratic: 1 + x₁ + x₂ + x₃ + x₁² + x₂² + x₃² + x₁x₂ + x₁x₃ + x₂x₃ |
| Parameters | p = 10, df_res = 7, df_LoF = 5, df_PE = 2 |

---

## Analysis phases

1. CCD design classification
2. Normality verification and transformations
3. Main effects and interactions (exploratory)
4. Correlations and preliminary residuals
5. Preliminary models (linear + interactions)
6. Full quadratic RSM fitting (ANOVA, coefficients, validation)
7. Effect interpretation (Pareto, 3D surfaces, pred vs obs)
8. Optimum localisation (Hessian analysis)
9. Multi-response optimisation (weighted desirability, sensitivity)
10. Per-experiment desirability with corrected bounds

---

## Results and figures

### Phase 2 — Normality and transformations

The Shapiro-Wilk test was applied to all nine original response variables. Variables that rejected normality (p < 0.05) were transformed using square root (√Y) or logarithm (log Y) depending on the observed skewness pattern. The histogram grid compares the original vs transformed distribution for each variable: a clear reduction in positive skewness is observed for Y₁, Y₂, Y₅, Y₈ and Y₉ after the SQRT transformation, and for Y₃ and Y₄ after LOG. Variables Y₆ and Y₇ (Cresol and Methional) show identical values across all experiments, suggesting a possible quantification issue or chromatographic co-elution.

---

### Phase 3.1 — Main effects plots

Each response variable has its own figure with three boxplots (one per factor, low vs high level), visualising whether the mean of Y changes appreciably when moving from level −1 to +1 of each coded factor. In Y₁ and Y₂, x₁ (extraction time) produces the largest median shift, whereas in Y₅ (Indole) the most marked effect corresponds to x₂ (desorption time), consistent with the significant interaction detected later.

---

### Phase 3.2 — Interaction plots

For each factor pair (x₁x₂, x₁x₃, x₂x₃), the mean of Y at the low and high level of one factor is plotted separately for each level of the other factor. Parallel lines indicate no interaction; crossing or diverging lines signal an active interaction. The x₁×x₂ interaction in Y₅ (Indole) is the most visible: the means cross, indicating that the effect of desorption on Indole depends strongly on the extraction time used.

---

### Phase 3.3 — Correlations and factor collinearity

The Pearson correlation matrix between coded factors x₁, x₂ and x₃ was computed. The low inter-factor correlation (|r| < 0.15 for all pairs) confirms the practical orthogonality of the CCD, a necessary condition for stable quadratic model coefficient estimates with low variance inflation.

---

### Phase 3 — Preliminary models: linear vs interactions

The coefficient figures (one per response Y) compare estimates from the pure linear model (β₁, β₂, β₃) against the model with first-order interactions (β₁₂, β₁₃, β₂₃), with 95 % confidence error bars. Terms whose interval does not cross zero are statistically significant at that level. This phase provides orientation prior to the quadratic fit, identifying which factors and interactions deserve attention without yet committing to the full model.

---

### Phase 4 — Quadratic RSM fit: global comparison

![RSM Comparison — R², F_reg, F_LoF](RSM_Resultados/Comparacion_RSM.png)

This three-panel figure summarises the goodness of fit for the nine quadratic models. The top panel shows adjusted R² and prediction R² (PRESS); the middle panel shows the regression F-statistic with the 5 % significance line; the bottom panel shows the Lack-of-Fit F-statistic. A well-fitted model will exhibit high R²_adj, significant F_reg, and non-significant F_LoF (the model captures curvature without overfitting). Y₁, Y₂ and Y₅ show the best fits, whereas Y₃ and Y₄ (%RSD) are the most difficult to model quadratically.

---

### Phase 5.1 — Pareto charts of effects

One Pareto chart per response variable (Y₁–Y₉) orders the nine quadratic model terms from largest to smallest absolute t-statistic, with a vertical line at the critical t value (α = 0.05, df = 7). Terms exceeding the line are statistically significant.

| Variable | Figure |
|---|---|
| Y₁ — Target Metabolites | ![Pareto Y1](RSM_Resultados/Y1_Pareto.png) |
| Y₂ — Total Metabolites | ![Pareto Y2](RSM_Resultados/Y2_Pareto.png) |
| Y₃ — %RSD All | ![Pareto Y3](RSM_Resultados/Y3_Pareto.png) |
| Y₄ — %RSD Target | ![Pareto Y4](RSM_Resultados/Y4_Pareto.png) |
| Y₅ — Indole/All | ![Pareto Y5](RSM_Resultados/Y5_Pareto.png) |
| Y₆ — Cresol/All | ![Pareto Y6](RSM_Resultados/Y6_Pareto.png) |
| Y₇ — Methional/All | ![Pareto Y7](RSM_Resultados/Y7_Pareto.png) |
| Y₈ — Carboxylic Acids/All | ![Pareto Y8](RSM_Resultados/Y8_Pareto.png) |
| Y₉ — Targets/Total | ![Pareto Y9](RSM_Resultados/Y9_Pareto.png) |

The linear terms of x₁ and x₂ dominate in Y₁ and Y₂, while in Y₅ the x₁x₂ interaction is the most prominent effect (|t| > t critical). The quadratic terms x₁² and x₂² are significant in several responses, confirming that surface curvature is real and not a design artefact.

---

### Phase 5.2 + 5.3 — 3D Response surfaces and contour maps

For each response variable, six panels are generated: three 3D surfaces (x₁x₂, x₁x₃ and x₂x₃ planes, with the third factor held at its central value) and their corresponding contour maps. Yellow/red regions in the contour maps indicate zones of high response value; densely grouped contours signal high local sensitivity of the response to the factors.

| Variable | 3D Surfaces + Contours |
|---|---|
| Y₁ | ![Surfaces Y1](RSM_Resultados/Y1_Superficies.png) |
| Y₂ | ![Surfaces Y2](RSM_Resultados/Y2_Superficies.png) |
| Y₃ | ![Surfaces Y3](RSM_Resultados/Y3_Superficies.png) |
| Y₄ | ![Surfaces Y4](RSM_Resultados/Y4_Superficies.png) |
| Y₅ | ![Surfaces Y5](RSM_Resultados/Y5_Superficies.png) |
| Y₆ | ![Surfaces Y6](RSM_Resultados/Y6_Superficies.png) |
| Y₇ | ![Surfaces Y7](RSM_Resultados/Y7_Superficies.png) |
| Y₈ | ![Surfaces Y8](RSM_Resultados/Y8_Superficies.png) |
| Y₉ | ![Surfaces Y9](RSM_Resultados/Y9_Superficies.png) |

In Y₅ (Indole), the x₁x₂ surface shows a saddle shape, consistent with the crossed interaction detected in the Pareto chart: at short extraction times, increasing desorption raises Indole; at long extraction times, the effect reverses.

---

### Phase 5.4 — Predicted vs Observed

![Predicted vs Observed — all Y](RSM_Resultados/PredVsObs_Todas.png)

A 3×3 panel with one scatter plot per response variable, where the x-axis shows values predicted by the quadratic model and the y-axis shows experimentally observed values. The diagonal reference line (predicted = observed) allows visual assessment of predictive capability. Models closest to the diagonal (Y₁, Y₂, Y₅) show lower residuals and higher R²_pred, while Y₃ and Y₄ show greater scatter around the diagonal, indicating limitations of the quadratic model in capturing %RSD variability.

---

### Phase 6 — Hessian analysis: critical point localisation

![Hessian analysis — critical points](RSM_Resultados/Fase6_Hessianos.png)

For each quadratic model, the system ∂Ŷ/∂xᵢ = 0 (i = 1, 2, 3) was solved analytically, yielding the stationary point in coded coordinates. The nature of that point (maximum, minimum or saddle point) is determined by evaluating the eigenvalues of the Hessian matrix (H = ∂²Ŷ/∂xᵢ∂xⱼ): all negative → maximum; all positive → minimum; mixed signs → saddle point. The figure shows, for each Y, the critical point coordinates and the type of stationary point. Critical points outside the experimental space (|xᵢ| > 1.68) must be interpreted with caution, as they lie in the model's extrapolation region.

---

### Phase 7.2 — Global multi-response optimum

![Global multi-response optimum](RSM_Resultados/Fase7_Optimo_Global.png)

Visualisation of the optimum point obtained by maximising the weighted global desirability function D (Derringer & Suich, 1980). A differential evolution algorithm was used for global search, followed by L-BFGS-B local refinement. The figure shows, for each response variable, the predicted value at the optimum (bar) versus the experimental range (shaded band), with the individual desirability dᵢ superimposed. The optimum lies at high extraction time and moderate-to-high desorption time, balancing metabolomic coverage (Y₁, Y₂) with aromatic selectivity (Y₅, Y₆, Y₇).

---

### Phase 7.1 — Individual desirabilities at the optimum

![Individual desirabilities](RSM_Resultados/Fase7_Deseabilidades.png)

Bar chart showing the individual desirability dᵢ ∈ [0,1] of each response evaluated at the global optimum point. The assigned weights reflect the analytical priority of the study: Y₁ (w=0.20) and Y₂ (w=0.15) carry the highest weight as general coverage responses; Y₅, Y₆ and Y₇ (w=0.15, 0.10, 0.10) represent the target aromatics; Y₃ and Y₄ (w=0.10 each) penalise imprecision. The shorter bars for Y₃/Y₄ reflect the difficulty of simultaneously minimising %RSD without compromising extraction yield.

---

### Phase 7.3 — Sensitivity analysis

![Optimum sensitivity analysis](RSM_Resultados/Fase7_Sensibilidad.png)

The L/T/U bounds of each specification were systematically perturbed by ±15 % and the weights wᵢ by ±50 %, evaluating the change in D_global under each scenario. The figure shows the relative variation of D with respect to each perturbation. The most sensitive responses (tallest bars) are those whose specification limits are closest to the experimental region, so small changes in L or T significantly shift the desirability function. The robustness of the optimum against moderate weight perturbations confirms that the solution is structurally stable.

---

### In-depth analysis — Y₅: Indole

![Y5 Surfaces — Indole (detailed analysis)](RSM_Resultados/Y5_Indol_Superficies.png)

![x₁×x₂ interaction for Y₅](RSM_Resultados/Y5_Indol_Interaccion.png)

Indole (Y₅) received an individual analysis given its relevance as a target aromatic marker and the presence of a significant x₁×x₂ interaction (p = 0.016). The 3D surface of the x₁x₂ plane shows a saddle morphology, indicating that the Indole optimum cannot be reached by independently maximising each factor. The interaction plot confirms the crossing: at low extraction time (x₁ = −1) Indole increases with desorption, while at high extraction time (x₁ = +1) the relationship reverses. The optimal condition for Y₅ is located at high extraction time and low desorption time, with incubation time at the central point.

---

### Per-experiment desirability — Corrected bounds

![Per-experiment desirability (original vs corrected)](RSM_Resultados/Desirabilidad_por_Experimento.png)

In the standard formulation, using L = observed minimum means that any experiment at that minimum receives dᵢ = 0, collapsing D_global to zero through the weighted geometric mean. The correction applied shifts the floor to L = μ − 2σ (for maximised responses) and the ceiling to U = μ + 2σ (for minimised responses), ensuring that no observed experiment automatically receives zero desirability. The top panel compares original vs corrected D_global for all 17 experiments; the bottom panel decomposes the individual desirability of the experiment with the highest D_global in the corrected version.

---

## Output files (Excel)

| File | Content |
|---|---|
| `RSM_Resultados/RSM_Completo_Todos_Y.xlsx` | Coefficients, ANOVA, metrics and residuals for Y₁–Y₉ |
| `RSM_Resultados/Resumen_Comparativo_RSM.xlsx` | Comparative table of R², F_reg, F_LoF for all models |
| `RSM_Resultados/Resultados_RSM_Fases567.xlsx` | Critical points, desirabilities, global optimum and sensitivity |

---

## Conclusions

Fitting quadratic RSM models on the 17-experiment Central Composite Design yields the following conclusions:

**1. Metabolomic coverage (Y₁, Y₂):** Extraction time (x₁) is the dominant factor. Both variables show models with R²_adj > 0.80 and significant F_reg, with the linear and quadratic effects of x₁ as the most influential terms according to the Pareto charts. The response surface has a dome shape shifted towards high x₁ values, suggesting that long extraction times (close to 120 min) favour metabolite detection.

**2. Instrumental precision (Y₃, Y₄):** The quadratic models for %RSD show the poorest fits (lowest R²_adj, F_LoF near the significance threshold), indicating that instrumental variability does not follow a simple quadratic structure in the studied experimental space. Hessian analysis classifies the critical points of Y₃ and Y₄ as saddle points or minima outside the experimental space, reinforcing the difficulty of optimising these variables.

**3. Target aromatics (Y₅ Indole, Y₆ Cresol, Y₇ Methional):** The relative fraction of Indole (Y₅) is the most tractable of the three, with a significant x₁×x₂ interaction generating a saddle-type surface in the extraction-desorption plane. Cresol and Methional show virtually identical values across all experiments (correlation r = 1.000), which may reflect chromatographic co-elution or a single shared release mechanism; this finding limits the independent optimisation of both variables.

**4. Multi-response optimisation:** The maximum global desirability (D ≈ 0.62–0.71 depending on the bounds version) is achieved at high extraction time (~110–120 min), moderate desorption time (~2 min) and incubation time close to the central point (~10 min). This solution balances the maximisation of Y₁, Y₂ and Y₅ with the minimisation of Y₃ and Y₄. The sensitivity analysis confirms that the optimum is robust against moderate perturbations in weights and specification bounds (D_global variation < 8 % for ±15 % perturbations in L/T/U).

**5. Methodological note:** Correcting the desirability bounds (L = μ − 2σ instead of L = observed minimum) is necessary in designs with few experiments, where the observed minimum is by definition reached by some design point, artificially collapsing its desirability to zero. The corrected version identifies Experiment 7 (x₁=+1, x₂=+1, x₃=−1) as the one with the highest observed D_global among the 17 experimental points.

---

## Requirements

```
Python >= 3.9
pandas
numpy
matplotlib
scipy
statsmodels
openpyxl
jupyter
```

Installation:

```bash
pip install pandas numpy matplotlib scipy statsmodels openpyxl jupyter
```

Execution:

```bash
jupyter notebook "Untitled-1.ipynb"
```

---

## Bibliographic references

1. **Box, G. E. P. & Wilson, K. B.** (1951). On the experimental attainment of optimum conditions. *Journal of the Royal Statistical Society, Series B*, 13(1), 1–45.

2. **Box, G. E. P. & Behnken, D. W.** (1960). Some new three level designs for the study of quantitative variables. *Technometrics*, 2(4), 455–475.

3. **Box, G. E. P. & Draper, N. R.** (1987). *Empirical Model-Building and Response Surfaces*. John Wiley & Sons, New York.

4. **Myers, R. H., Montgomery, D. C. & Anderson-Cook, C. M.** (2016). *Response Surface Methodology: Process and Product Optimization Using Designed Experiments* (4th ed.). John Wiley & Sons, Hoboken, NJ.

5. **Montgomery, D. C.** (2017). *Design and Analysis of Experiments* (9th ed.). John Wiley & Sons, Hoboken, NJ.

6. **Derringer, G. & Suich, R.** (1980). Simultaneous optimization of several response variables. *Journal of Quality Technology*, 12(4), 214–219.

7. **Harrington, E. C.** (1965). The desirability function. *Industrial Quality Control*, 21(10), 494–498.

8. **Shapiro, S. S. & Wilk, M. B.** (1965). An analysis of variance test for normality (complete samples). *Biometrika*, 52(3–4), 591–611.

9. **Cook, R. D.** (1977). Detection of influential observation in linear regression. *Technometrics*, 19(1), 15–18.

10. **Storn, R. & Price, K.** (1997). Differential evolution — A simple and efficient heuristic for global optimization over continuous spaces. *Journal of Global Optimization*, 11(4), 341–359.

11. **Seabold, S. & Perktold, J.** (2010). Statsmodels: Econometric and statistical modeling with Python. *Proceedings of the 9th Python in Science Conference (SciPy 2010)*, 57–61.

12. **Virtanen, P. et al.** (2020). SciPy 1.0: Fundamental algorithms for scientific computing in Python. *Nature Methods*, 17, 261–272.

---

*Final Degree Project · 2025–2026*
