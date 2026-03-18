# RSM — Optimización de Extracción Aromática

> Trabajo de Fin de Grado · Análisis completo mediante Metodología de Superficie de Respuesta (RSM) con Diseño Compuesto Central (CCD)

---

## Descripción

Optimización multirrespuesta de un proceso de extracción aromática mediante RSM y Diseño Compuesto Central (CCD, 17 exp., 3 factores). Incluye ajuste cuadrático completo, análisis Hessiano, superficies de respuesta 3D y función de deseabilidad ponderada (Derringer & Suich, 1980).

---

## Estructura del experimento

| Elemento | Detalle |
|---|---|
| Diseño | Diseño Compuesto Central (CCD) rotable |
| Experimentos | 17 (8 factoriales + 6 axiales + 3 réplicas centrales) |
| Factores | x₁: t extracción (30–120 min), x₂: t desorción (1–3 min), x₃: t incubación (5–15 min) |
| Respuestas | Y₁–Y₉ (metabolitos diana, totales, %RSD, Indol, Cresol, Metional, Ac. Carboxílicos, Dianas/Total) |
| Modelo | Cuadrático completo: 1 + x₁ + x₂ + x₃ + x₁² + x₂² + x₃² + x₁x₂ + x₁x₃ + x₂x₃ |
| Parámetros | p = 10, df_res = 7, df_LoF = 5, df_PE = 2 |

---

## Fases del análisis

1. Clasificación del diseño CCD
2. Verificación de normalidad y transformaciones
3. Efectos principales e interacciones (exploración)
4. Correlaciones y residuos preliminares
5. Modelos preliminares (lineal + interacciones)
6. Ajuste RSM cuadrático completo (ANOVA, coeficientes, validación)
7. Interpretación de efectos (Pareto, superficies 3D, pred vs obs)
8. Localización del óptimo (análisis Hessiano)
9. Optimización multirrespuesta (deseabilidad ponderada, sensibilidad)
10. Deseabilidad por experimento con límites corregidos

---

## Resultados y figuras

### Fase 2 — Normalidad y transformaciones

Se aplicó el test de Shapiro-Wilk sobre las nueve variables de respuesta originales. Aquellas que rechazaron normalidad (p < 0.05) fueron transformadas mediante raíz cuadrada (√Y) o logaritmo (log Y) según el patrón de asimetría observado. El grid de histogramas permite comparar la distribución original frente a la transformada para cada variable: se observa una reducción clara de la asimetría positiva en Y₁, Y₂, Y₅, Y₈ e Y₉ tras la transformación SQRT, y en Y₃ e Y₄ tras LOG. Las variables Y₆ y Y₇ (Cresol y Metional) presentan valores idénticos en todos los experimentos, lo que sugiere un posible problema de cuantificación o coelución cromatográfica.

---

### Fase 3.1 — Gráficos de efectos principales

Cada variable de respuesta tiene su propia figura con tres boxplots (uno por factor, nivel bajo vs alto), que permiten visualizar si el valor medio de Y varía de forma apreciable al pasar del nivel −1 al +1 de cada factor codificado. En Y₁ y Y₂ se observa que x₁ (t extracción) produce el mayor desplazamiento de la mediana, mientras que en Y₅ (Indol) el efecto más marcado corresponde a x₂ (t desorción), coherente con la interacción significativa detectada posteriormente.

---

### Fase 3.2 — Gráficos de interacción

Para cada par de factores (x₁x₂, x₁x₃, x₂x₃) se representan las medias de Y a nivel bajo y alto de un factor, trazadas por separado para cada nivel del otro factor. Las líneas paralelas indican ausencia de interacción; las líneas cruzadas o divergentes señalan interacción activa. La interacción x₁×x₂ en Y₅ (Indol) es la más visible: las medias se cruzan, indicando que el efecto de la desorción sobre el Indol depende fuertemente del tiempo de extracción empleado.

---

### Fase 3.3 — Correlaciones y colinealidad entre factores

![Correlaciones y residuos](RSM_Resultados/Y1/Y1_Pareto.png)

Se calculó la matriz de correlación de Pearson entre los factores codificados x₁, x₂ y x₃. La baja correlación entre factores (|r| < 0.15 en todos los pares) confirma la ortogonalidad práctica del diseño CCD, condición necesaria para que los estimadores de los coeficientes del modelo cuadrático sean estables y poco inflados por colinealidad.

---

### Fase 3 — Modelos preliminares: lineal vs interacciones

Las figuras de coeficientes (una por variable Y) comparan los estimadores del modelo lineal puro (β₁, β₂, β₃) frente al modelo con interacciones (β₁₂, β₁₃, β₂₃). Se usan barras de error al 95 % de confianza. Los términos cuyo intervalo no cruza el cero son estadísticamente significativos a ese nivel. Esta fase sirve de orientación previa al ajuste cuadrático: permite identificar qué factores e interacciones merecen atención sin comprometerse aún con el modelo completo.

---

### Fase 4 — Ajuste RSM cuadrático: comparativa global

![Comparación RSM — R², F_reg, F_LoF](RSM_Resultados/Comparacion_RSM.png)

Esta figura de tres paneles resume la bondad de ajuste de los nueve modelos cuadráticos. El panel superior muestra el R² ajustado y el R² de predicción (PRESS); el central, el valor del estadístico F de regresión con la línea de significancia al 5 %; el inferior, el F de Lack-of-Fit. Un modelo bien ajustado presentará R²_adj elevado, F_reg significativo y F_LoF no significativo (el modelo captura la curvatura sin sobreajustar). Se observa que Y₁, Y₂ e Y₅ presentan los mejores ajustes, mientras que Y₃ e Y₄ (%RSD) son los más difíciles de modelar cuadráticamente.

---

### Fase 5.1 — Diagramas de Pareto de efectos

Un diagrama de Pareto por cada variable de respuesta (Y₁–Y₉) ordena los nueve términos del modelo cuadrático de mayor a menor valor absoluto del estadístico t, con una línea vertical en t crítico (α = 0.05, gl = 7). Los términos que superan la línea son estadísticamente significativos.

| Variable | Figura |
|---|---|
| Y₁ — Metabolitos Diana | ![Pareto Y1](RSM_Resultados/Y1_Pareto.png) |
| Y₂ — Metabolitos Totales | ![Pareto Y2](RSM_Resultados/Y2_Pareto.png) |
| Y₃ — %RSD Todos | ![Pareto Y3](RSM_Resultados/Y3_Pareto.png) |
| Y₄ — %RSD Diana | ![Pareto Y4](RSM_Resultados/Y4_Pareto.png) |
| Y₅ — Indol/Todos | ![Pareto Y5](RSM_Resultados/Y5_Pareto.png) |
| Y₆ — Cresol/Todos | ![Pareto Y6](RSM_Resultados/Y6_Pareto.png) |
| Y₇ — Metional/Todos | ![Pareto Y7](RSM_Resultados/Y7_Pareto.png) |
| Y₈ — Ác. Carboxílicos/Todos | ![Pareto Y8](RSM_Resultados/Y8_Pareto.png) |
| Y₉ — Dianas/Totales | ![Pareto Y9](RSM_Resultados/Y9_Pareto.png) |

Los términos lineales de x₁ y x₂ dominan en Y₁ e Y₂, mientras que en Y₅ la interacción x₁x₂ es el efecto más destacado (|t| > t crítico). Los términos cuadráticos x₁² y x₂² aparecen significativos en varias respuestas, confirmando que la curvatura de la superficie es real y no un artefacto del diseño.

---

### Fase 5.2 + 5.3 — Superficies de respuesta 3D y mapas de contorno

Para cada variable de respuesta se generan seis paneles: tres superficies 3D (planos x₁x₂, x₁x₃ y x₂x₃, con el tercer factor en su valor central) y sus correspondientes mapas de contorno. Las regiones coloreadas en amarillo/rojo en los mapas de contorno indican zonas de valor elevado de la respuesta; los contornos densamente agrupados señalan alta sensibilidad local de la respuesta a los factores.

| Variable | Superficies 3D + Contornos |
|---|---|
| Y₁ | ![Superficies Y1](RSM_Resultados/Y1_Superficies.png) |
| Y₂ | ![Superficies Y2](RSM_Resultados/Y2_Superficies.png) |
| Y₃ | ![Superficies Y3](RSM_Resultados/Y3_Superficies.png) |
| Y₄ | ![Superficies Y4](RSM_Resultados/Y4_Superficies.png) |
| Y₅ | ![Superficies Y5](RSM_Resultados/Y5_Superficies.png) |
| Y₆ | ![Superficies Y6](RSM_Resultados/Y6_Superficies.png) |
| Y₇ | ![Superficies Y7](RSM_Resultados/Y7_Superficies.png) |
| Y₈ | ![Superficies Y8](RSM_Resultados/Y8_Superficies.png) |
| Y₉ | ![Superficies Y9](RSM_Resultados/Y9_Superficies.png) |

En Y₅ (Indol) la superficie x₁x₂ presenta forma de silla de montar, coherente con la interacción cruzada detectada en el Pareto: a tiempos de extracción cortos, incrementar la desorción aumenta el Indol; a tiempos largos, el efecto se invierte.

---

### Fase 5.4 — Predicción vs Observado

![Predicción vs Observado — todas las Y](RSM_Resultados/PredVsObs_Todas.png)

Panel 3×3 con un diagrama de dispersión por variable de respuesta, donde el eje x representa los valores predichos por el modelo cuadrático y el eje y los valores observados experimentalmente. La línea diagonal de referencia (predicho = observado) permite evaluar visualmente la capacidad predictiva del modelo. Los modelos más cercanos a la diagonal (Y₁, Y₂, Y₅) presentan menores residuos y mayor R²_pred, mientras que Y₃ e Y₄ muestran mayor dispersión en torno a la diagonal, indicando limitaciones del modelo cuadrático para capturar la variabilidad del %RSD.

---

### Fase 6 — Análisis Hessiano: localización del punto crítico

![Análisis Hessiano — puntos críticos](RSM_Resultados/Fase6_Hessianos.png)

Para cada modelo cuadrático se resolvió analíticamente el sistema ∂Ŷ/∂xᵢ = 0 (i = 1, 2, 3), obteniendo el punto estacionario en coordenadas codificadas. La naturaleza de ese punto (máximo, mínimo o punto silla) se determina evaluando los autovalores de la matriz Hessiana (H = ∂²Ŷ/∂xᵢ∂xⱼ): todos negativos → máximo; todos positivos → mínimo; signos mixtos → punto silla. La figura muestra, para cada Y, las coordenadas del punto crítico y el tipo de estacionario. Puntos críticos fuera del espacio experimental (|xᵢ| > 1.68) deben interpretarse con precaución, ya que se encuentran en zona de extrapolación del modelo.

---

### Fase 7.2 — Óptimo global multirrespuesta

![Óptimo global multirrespuesta](RSM_Resultados/Fase7_Optimo_Global.png)

Visualización del punto óptimo obtenido mediante la maximización de la función de deseabilidad global D ponderada (Derringer & Suich, 1980). Se empleó un algoritmo de evolución diferencial como búsqueda global seguido de un refinamiento local L-BFGS-B. La figura muestra, para cada variable de respuesta, el valor predicho en el óptimo (barra) frente al rango experimental (banda sombreada), con la deseabilidad individual dᵢ sobreimpresa. El óptimo se localiza en condiciones de t extracción elevada y t desorción moderada-alta, equilibrando la cobertura metabolómica (Y₁, Y₂) con la selectividad aromática (Y₅, Y₆, Y₇).

---

### Fase 7.1 — Deseabilidades individuales en el óptimo

![Deseabilidades individuales](RSM_Resultados/Fase7_Deseabilidades.png)

Diagrama de barras con la deseabilidad individual dᵢ ∈ [0,1] de cada respuesta evaluada en el punto óptimo global. Los pesos asignados reflejan la prioridad analítica del estudio: Y₁ (w=0.20) y Y₂ (w=0.15) tienen mayor peso por ser las respuestas de cobertura general; Y₅, Y₆ y Y₇ (w=0.15, 0.10, 0.10) representan los aromáticos diana; Y₃ e Y₄ (w=0.10 cada una) penalizan la imprecisión. Las barras más cortas en Y₃/Y₄ reflejan la dificultad de minimizar simultáneamente el %RSD sin comprometer la extracción.

---

### Fase 7.3 — Análisis de sensibilidad

![Análisis de sensibilidad del óptimo](RSM_Resultados/Fase7_Sensibilidad.png)

Se perturbaron sistemáticamente los límites L/T/U de cada especificación en ±15 % y los pesos wᵢ en ±50 %, evaluando la variación de D_global en cada escenario. La figura muestra la variación relativa de D respecto a cada perturbación. Las respuestas más sensibles (mayor altura de barra) son aquellas cuyos límites de especificación están próximos a la región experimental, de modo que pequeños cambios en L o T desplazan significativamente la función de deseabilidad. La robustez del óptimo frente a perturbaciones moderadas de los pesos confirma que la solución es estructuralmente estable.

---

### Análisis en profundidad — Y₅: Indol

![Superficies Y5 — Indol (análisis detallado)](RSM_Resultados/Y5_Indol_Superficies.png)

![Interacción x₁×x₂ para Y₅](RSM_Resultados/Y5_Indol_Interaccion.png)

El Indol (Y₅) recibió un análisis individualizado dada su relevancia como marcador aromático diana y la presencia de una interacción significativa x₁×x₂ (p = 0.016). La superficie 3D del plano x₁x₂ presenta morfología de silla, indicando que el óptimo de Indol no puede alcanzarse maximizando independientemente cada factor. El gráfico de interacción confirma el cruce: a t extracción baja (x₁ = −1) el Indol aumenta con la desorción, mientras que a t extracción alta (x₁ = +1) la relación se invierte. La condición óptima para Y₅ se sitúa en t extracción alta y t desorción baja, con t incubación en el punto central.

---

### Deseabilidad por experimento — Corrección de límites

![Deseabilidad por experimento (original vs corregida)](RSM_Resultados/Desirabilidad_por_Experimento.png)

En la formulación estándar, usar L = mínimo observado implica que cualquier experimento en ese mínimo recibe dᵢ = 0, lo que colapsa D_global a cero por efecto de la media geométrica ponderada. La corrección aplicada traslada el suelo a L = μ − 2σ (para respuestas a maximizar) y el techo a U = μ + 2σ (para respuestas a minimizar), garantizando que ningún experimento observado recibe automáticamente deseabilidad nula. El panel superior compara D_global original vs corregida para los 17 experimentos; el panel inferior descompone la deseabilidad individual del experimento con mayor D_global en la versión corregida.

---

## Archivos de salida (Excel)

| Archivo | Contenido |
|---|---|
| `RSM_Resultados/RSM_Completo_Todos_Y.xlsx` | Coeficientes, ANOVA, métricas y residuos para Y₁–Y₉ |
| `RSM_Resultados/Resumen_Comparativo_RSM.xlsx` | Tabla comparativa de R², F_reg, F_LoF para todos los modelos |
| `RSM_Resultados/Resultados_RSM_Fases567.xlsx` | Puntos críticos, deseabilidades, óptimo global y sensibilidad |

---

## Conclusiones

El ajuste de modelos cuadráticos RSM sobre el Diseño Compuesto Central de 17 experimentos permite extraer las siguientes conclusiones:

**1. Cobertura metabolómica (Y₁, Y₂):** El tiempo de extracción (x₁) es el factor dominante. Ambas variables presentan modelos con R²_adj > 0.80 y F_reg significativo, con efectos lineales y cuadráticos de x₁ como términos más influyentes según el diagrama de Pareto. La superficie de respuesta tiene forma de cúpula desplazada hacia valores altos de x₁, lo que sugiere que tiempos de extracción largos (próximos a 120 min) favorecen la detección de metabolitos.

**2. Precisión instrumental (Y₃, Y₄):** Los modelos cuadráticos para %RSD presentan los peores ajustes (R²_adj más bajos, F_LoF en el límite de significancia), lo que indica que la variabilidad instrumental no sigue una estructura cuadrática simple en el espacio experimental estudiado. El análisis Hessiano clasifica los puntos críticos de Y₃ e Y₄ como puntos silla o mínimos fuera del espacio experimental, reforzando la dificultad de optimizar estas variables.

**3. Aromáticos diana (Y₅ Indol, Y₆ Cresol, Y₇ Metional):** La fracción relativa de Indol (Y₅) es la más modelable de las tres, con una interacción x₁×x₂ significativa que genera una superficie de tipo silla en el plano extracción-desorción. Cresol y Metional presentan valores prácticamente idénticos en todos los experimentos (correlación r = 1.000), lo que podría reflejar coelución cromatográfica o un único mecanismo de liberación compartido; este hallazgo limita la optimización independiente de ambas variables.

**4. Optimización multirrespuesta:** La deseabilidad global máxima (D ≈ 0.62–0.71 según la versión de límites) se alcanza en condiciones de t extracción elevada (~110–120 min), t desorción moderada (~2 min) y t incubación próxima al punto central (~10 min). Esta solución equilibra la maximización de Y₁, Y₂ e Y₅ con la minimización de Y₃ e Y₄. El análisis de sensibilidad confirma que el óptimo es robusto frente a perturbaciones moderadas en los pesos y los límites de especificación (variación de D_global < 8 % ante perturbaciones del ±15 % en L/T/U).

**5. Consideración metodológica:** La corrección de los límites de deseabilidad (L = μ − 2σ en lugar de L = mínimo observado) es necesaria en diseños con pocos experimentos, donde el mínimo observado es alcanzado por definición por algún punto del diseño, colapsando artificialmente su deseabilidad a cero. La versión corregida identifica el Experimento 7 (x₁=+1, x₂=+1, x₃=−1) como el de mayor D_global observada entre los 17 puntos experimentales.

---

## Requisitos

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

Instalación:

```bash
pip install pandas numpy matplotlib scipy statsmodels openpyxl jupyter
```

Ejecución:

```bash
jupyter notebook "Untitled-1.ipynb"
```

---

## Referencias bibliográficas

1. **Box, G. E. P. & Wilson, K. B.** (1951). On the experimental attainment of optimum conditions. *Journal of the Royal Statistical Society, Series B*, 13(1), 1–45.

2. **Box, G. E. P. & Behnken, D. W.** (1960). Some new three level designs for the study of quantitative variables. *Technometrics*, 2(4), 455–475.

3. **Box, G. E. P. & Draper, N. R.** (1987). *Empirical Model-Building and Response Surfaces*. John Wiley & Sons, New York.

4. **Myers, R. H., Montgomery, D. C. & Anderson-Cook, C. M.** (2016). *Response Surface Methodology: Process and Product Optimization Using Designed Experiments* (4ª ed.). John Wiley & Sons, Hoboken, NJ.

5. **Montgomery, D. C.** (2017). *Design and Analysis of Experiments* (9ª ed.). John Wiley & Sons, Hoboken, NJ.

6. **Derringer, G. & Suich, R.** (1980). Simultaneous optimization of several response variables. *Journal of Quality Technology*, 12(4), 214–219.

7. **Harrington, E. C.** (1965). The desirability function. *Industrial Quality Control*, 21(10), 494–498.

8. **Shapiro, S. S. & Wilk, M. B.** (1965). An analysis of variance test for normality (complete samples). *Biometrika*, 52(3–4), 591–611.

9. **Cook, R. D.** (1977). Detection of influential observation in linear regression. *Technometrics*, 19(1), 15–18.

10. **Storn, R. & Price, K.** (1997). Differential evolution — A simple and efficient heuristic for global optimization over continuous spaces. *Journal of Global Optimization*, 11(4), 341–359.

11. **Seabold, S. & Perktold, J.** (2010). Statsmodels: Econometric and statistical modeling with Python. *Proceedings of the 9th Python in Science Conference (SciPy 2010)*, 57–61.

12. **Virtanen, P. et al.** (2020). SciPy 1.0: Fundamental algorithms for scientific computing in Python. *Nature Methods*, 17, 261–272.

---

*Trabajo de Fin de Grado · 2025–2026*
