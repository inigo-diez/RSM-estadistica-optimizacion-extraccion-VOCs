# Optimización Multirrespuesta de un Proceso de Extracción Aromática mediante Metodología de Superficie de Respuesta (RSM)

> **Trabajo de Fin de Grado** · Análisis estadístico experimental completo con Diseño Compuesto Central (CCD) y 9 variables de respuesta.

---

## Índice

1. [Descripción del proyecto](#1-descripción-del-proyecto)
2. [Diseño experimental](#2-diseño-experimental)
3. [Variables del sistema](#3-variables-del-sistema)
4. [Estructura del notebook](#4-estructura-del-notebook)
5. [Figuras y tablas generadas — explicación detallada](#5-figuras-y-tablas-generadas--explicación-detallada)
   - 5.1 [Clasificación del CCD](#51-clasificación-del-ccd)
   - 5.2 [Verificación de normalidad y transformaciones](#52-verificación-de-normalidad-y-transformaciones)
   - 5.3 [Efectos principales (Main Effects Plots)](#53-efectos-principales-main-effects-plots)
   - 5.4 [Gráficos de interacción](#54-gráficos-de-interacción)
   - 5.5 [Correlaciones y residuos preliminares](#55-correlaciones-y-residuos-preliminares)
   - 5.6 [Modelos preliminares (Fase 3)](#56-modelos-preliminares-fase-3)
   - 5.7 [Ajuste RSM cuadrático completo (Fase 4)](#57-ajuste-rsm-cuadrático-completo-fase-4)
   - 5.8 [Análisis en profundidad de Y₅ — Indol](#58-análisis-en-profundidad-de-y₅--indol)
   - 5.9 [Interpretación de efectos: Pareto, Superficies, Pred vs Obs (Fase 5)](#59-interpretación-de-efectos-pareto-superficies-pred-vs-obs-fase-5)
   - 5.10 [Localización del óptimo — análisis Hessiano (Fase 6)](#510-localización-del-óptimo--análisis-hessiano-fase-6)
   - 5.11 [Optimización multirrespuesta — deseabilidad (Fase 7)](#511-optimización-multirrespuesta--deseabilidad-fase-7)
   - 5.12 [Deseabilidad por experimento con límites ampliados](#512-deseabilidad-por-experimento-con-límites-ampliados)
6. [Conclusiones finales](#6-conclusiones-finales)
7. [Archivos del repositorio](#7-archivos-del-repositorio)
8. [Requisitos](#8-requisitos)
9. [Referencias bibliográficas](#9-referencias-bibliográficas)

---

## 1. Descripción del proyecto

Este trabajo aplica la **Metodología de Superficie de Respuesta (RSM)** para modelar y optimizar simultáneamente nueve variables de respuesta que caracterizan la calidad aromática de un proceso de extracción. El objetivo central es identificar las condiciones operativas —tiempo de extracción, tiempo de desorción y tiempo de incubación— que maximizan la recuperación de compuestos diana (Indol, Cresol, Metional y ácidos carboxílicos) minimizando la variabilidad del método.

El análisis se estructura en siete fases progresivas: desde la verificación de supuestos distribucionales hasta la optimización global mediante la función de deseabilidad ponderada de Derringer y Suich (1980), pasando por el ajuste cuadrático completo, la localización analítica del óptimo mediante el análisis del Hessiano y la evaluación de la robustez del optimum mediante análisis de sensibilidad.

---

## 2. Diseño experimental

Se empleó un **Diseño Compuesto Central (CCD)** con tres factores y 17 experimentos, distribuidos en tres bloques:

| Tipo de punto | N.º experimentos | Valores codificados |
|---------------|-----------------|---------------------|
| Factoriales   | 8               | xᵢ = ±1            |
| Axiales (estrella) | 6          | xᵢ = ±α (α ≈ 1.68) |
| Centrales (réplicas) | 3        | xᵢ = 0             |

Las réplicas centrales (experimentos 14, 15 y 16) permiten estimar el **Error Puro** con 2 grados de libertad, lo que hace posible el test de Falta de Ajuste (Lack-of-Fit) independiente del error residual.

### Codificación de factores

| Factor real | Unidad | Centro (C) | Paso (S) | Variable codificada |
|-------------|--------|-----------|---------|---------------------|
| Tiempo de extracción | min | 75 | 45 | x₁ = (t − 75) / 45 |
| Tiempo de desorción  | min | 2  | 1  | x₂ = (t − 2) / 1   |
| Tiempo de incubación | min | 10 | 5  | x₃ = (t − 10) / 5  |

La codificación ortogonaliza los factores, elimina la correlación entre términos lineales y cuadráticos, y facilita la comparación directa de coeficientes como medida del efecto relativo de cada factor.

---

## 3. Variables del sistema

### Factores (variables independientes)

| Símbolo | Descripción | Rango real |
|---------|-------------|-----------|
| x₁ | Tiempo de extracción | 30 – 120 min |
| x₂ | Tiempo de desorción  | 1 – 3 min    |
| x₃ | Tiempo de incubación | 5 – 15 min   |

### Respuestas (variables dependientes)

| Símbolo | Descripción | Objetivo |
|---------|-------------|---------|
| Y₁ | N.º de metabolitos diana detectados | Maximizar |
| Y₂ | N.º total de metabolitos detectados | Maximizar |
| Y₃ | %RSD sobre todos los metabolitos | Minimizar |
| Y₄ | %RSD sobre metabolitos diana | Minimizar |
| Y₅ | Fracción relativa Indol/Todos | Maximizar |
| Y₆ | Fracción relativa Cresol/Todos | Maximizar |
| Y₇ | Fracción relativa Metional/Todos | Maximizar |
| Y₈ | Fracción relativa Ácidos carboxílicos/Todos | Maximizar |
| Y₉ | Fracción Dianas/Totales | Maximizar |

---

## 4. Estructura del notebook

```
Untitled-1.ipynb
│
├── Sección 1  — Clasificación CCD y verificación de normalidad
├── Sección 2  — Efectos principales e interacciones
├── Sección 3  — Correlaciones y residuos preliminares
├── Sección 4  — Modelos preliminares (lineal y con interacciones)
├── Sección 5  — Ajuste RSM cuadrático completo (Fases 4.1–4.6)
├── Sección 6  — Análisis en profundidad de Y₅ (Indol)
├── Fase 5     — Pareto de efectos · Superficies 3D · Pred vs Obs
├── Fase 6     — Punto crítico y diagnóstico Hessiano
├── Fase 7     — Optimización multirrespuesta (deseabilidad)
└── Corrección — Deseabilidad por experimento con límites ampliados
```

---

## Flujo de trabajo

```
┌─────────────────────────────────────────────────────────────┐
│                     DATOS DE ENTRADA                        │
│  Variables_y.xlsx · datos_transformados.xlsx                │
│  Diseño Experimental.xlsx  (17 exp., 3 factores, 9 resp.)   │
└────────────────────────┬────────────────────────────────────┘
                         │
                         ▼
┌─────────────────────────────────────────────────────────────┐
│  PASO 1 — Verificación de supuestos                         │
│  · Test Shapiro-Wilk por variable                           │
│  · Transformaciones √Y / log(Y) si p < 0.05                │
│  · Ortogonalidad de factores (|r| < 0.15)                   │
└────────────────────────┬────────────────────────────────────┘
                         │
                         ▼
┌─────────────────────────────────────────────────────────────┐
│  PASO 2 — Exploración visual                                │
│  · Main Effects Plots (9 variables × 3 factores)            │
│  · Interaction Plots  (9 variables × 3 pares)               │
│  · Residuos y colinealidad de modelos lineales              │
└────────────────────────┬────────────────────────────────────┘
                         │
                         ▼
┌─────────────────────────────────────────────────────────────┐
│  PASO 3 — Modelos preliminares                              │
│  · Modelo lineal puro: Y = β₀ + β₁x₁ + β₂x₂ + β₃x₃        │
│  · Modelo con interacciones: + β₁₂x₁x₂ + β₁₃x₁x₃ + β₂₃x₂x₃│
│  · Comparativa R² y términos significativos                 │
└────────────────────────┬────────────────────────────────────┘
                         │
                         ▼
┌─────────────────────────────────────────────────────────────┐
│  PASO 4 — Ajuste RSM cuadrático completo (modelo central)   │
│  · OLS sobre modelo de 10 parámetros (statsmodels)          │
│  · ANOVA: F_reg, F_LoF, Error Puro (réplicas centrales)     │
│  · Tabla de coeficientes con p-valores                      │
│  · Métricas: R², R²_adj, RMSE, PRESS                        │
└────────────────────────┬────────────────────────────────────┘
                         │
                         ▼
┌─────────────────────────────────────────────────────────────┐
│  PASO 5 — Interpretación de efectos                         │
│  · Pareto de |t| por variable (efectos significativos)      │
│  · Superficies 3D + mapas de contorno (3 planos × 9 resp.)  │
│  · Predicho vs Observado + RMSE visual                      │
└────────────────────────┬────────────────────────────────────┘
                         │
                         ▼
┌─────────────────────────────────────────────────────────────┐
│  PASO 6 — Localización del óptimo individual                │
│  · Punto crítico: ∂Ŷ/∂xᵢ = 0  →  x* = −½ B⁻¹ b            │
│  · Análisis Hessiano: eigenvalores → máx / mín / silla      │
└────────────────────────┬────────────────────────────────────┘
                         │
                         ▼
┌─────────────────────────────────────────────────────────────┐
│  PASO 7 — Optimización multirrespuesta                      │
│  · Deseabilidad individual dᵢ ∈ [0,1] (Derringer & Suich)  │
│  · Deseabilidad global D = media geométrica ponderada       │
│  · Búsqueda global: evolución diferencial + L-BFGS-B        │
│  · Análisis de sensibilidad: perturbación ±15% en L/T/U/w  │
└────────────────────────┬────────────────────────────────────┘
                         │
                         ▼
┌─────────────────────────────────────────────────────────────┐
│  CORRECCIÓN METODOLÓGICA                                    │
│  · L = μ − 2σ  (en lugar de L = mínimo observado)          │
│  · Deseabilidad D_global por cada uno de los 17 experimentos│
│  · Identificación del experimento óptimo observado          │
└─────────────────────────────────────────────────────────────┘
                         │
                         ▼
┌─────────────────────────────────────────────────────────────┐
│                    SALIDAS                                  │
│  Graficos/  (63 figuras PNG)                                │
│  RSM_Completo_Todos_Y.xlsx                                  │
│  Resumen_Comparativo_RSM.xlsx                               │
│  Resultados_RSM_Fases567.xlsx                               │
└─────────────────────────────────────────────────────────────┘
```

---

## 5. Figuras y tablas generadas — explicación detallada

### 5.1 Clasificación del CCD

**Tabla interna del notebook** que clasifica los 17 experimentos en sus tres categorías (factoriales, axiales, centrales) con los valores codificados de x₁, x₂ y x₃. Esta clasificación es el punto de partida del análisis: confirma que el diseño cumple los requisitos de un CCD válido —ortogonalidad aproximada, estimabilidad de todos los términos cuadráticos y existencia de réplicas para el test de Lack-of-Fit.

El hecho de que x₃ tome el valor ±0.82 en los puntos factoriales (en lugar del ±1 teórico) indica que el diseño fue construido con α = 1.68 bajo el criterio de **rotabilidad** (α = 2^(k/4)), lo que garantiza que la varianza de predicción es constante en puntos equidistantes del centro.

---

### 5.2 Verificación de normalidad y transformaciones

**Figura:** Grid 9×2 de histogramas (distribución original vs. transformada) con curvas de densidad normal ajustada.

![Normalidad — histogramas original vs. transformado](Graficos/normalidad_histogramas.png)

**Tabla:** Resumen de normalidad con p-valores Shapiro-Wilk, indicador booleano de normalidad (p > 0.05) y transformación aplicada.

El test de **Shapiro-Wilk** (Shapiro & Wilk, 1965) es el más adecuado para muestras pequeñas (n = 17). Cuando la hipótesis de normalidad se rechaza, se evalúan transformaciones de la familia Box-Cox (Box & Cox, 1964): raíz cuadrada √Y para datos de conteo y logarítmica log(Y) para datos con varianza proporcional a la media. La elección de la transformación se basa en el criterio de máxima log-verosimilitud del parámetro λ.

La normalidad de los residuos es un supuesto de los modelos OLS sobre los que se construye el RSM; su verificación en la variable original sirve como diagnóstico previo antes del ajuste formal.

---

### 5.3 Efectos principales (Main Effects Plots)

**Figura:** 9 paneles individuales (uno por variable de respuesta). Cada panel contiene 3 boxplots — uno por factor — con los puntos experimentales sobreimpresos. El eje horizontal distingue el nivel bajo (−1), central (0) y alto (+1) de cada factor codificado.

| Y₁ | Y₂ | Y₃ |
|:--:|:--:|:--:|
| ![Efectos Y1](Graficos/efectos_Y1_MetabDiana.png) | ![Efectos Y2](Graficos/efectos_Y2_MetabTotales.png) | ![Efectos Y3](Graficos/efectos_Y3_RSD_Todos.png) |

| Y₄ | Y₅ | Y₆ |
|:--:|:--:|:--:|
| ![Efectos Y4](Graficos/efectos_Y4_RSD_Diana.png) | ![Efectos Y5](Graficos/efectos_Y5_Indol.png) | ![Efectos Y6](Graficos/efectos_Y6_Cresol.png) |

| Y₇ | Y₈ | Y₉ |
|:--:|:--:|:--:|
| ![Efectos Y7](Graficos/efectos_Y7_Metional.png) | ![Efectos Y8](Graficos/efectos_Y8_AcCarboxilicos.png) | ![Efectos Y9](Graficos/efectos_Y9_DianasTotales.png) |

**Tabla:** Factor de mayor impacto por variable, calculado como el factor que maximiza |Δ| = |media(nivel alto) − media(nivel bajo)|.

Los Main Effects Plots permiten una primera lectura cualitativa del efecto de cada factor de forma aislada, manteniendo los otros factores promediados. Son especialmente útiles para detectar efectos no lineales: si la respuesta en el nivel central está por encima (o por debajo) de la línea que une los extremos, hay evidencia de curvatura, que en el modelo RSM cuadrático queda capturada por los coeficientes β₁₁, β₂₂, β₃₃.

---

### 5.4 Gráficos de interacción

**Figura:** 9 × 3 paneles (una fila por variable de respuesta, una columna por par de factores: x₁×x₂, x₁×x₃, x₂×x₃). Cada panel muestra la respuesta media en nivel bajo y alto de un factor, diferenciada por el nivel del factor cruzado.

| Y₁ | Y₂ | Y₃ |
|:--:|:--:|:--:|
| ![Interacción Y1](Graficos/interaccion_Y1_MetabDiana.png) | ![Interacción Y2](Graficos/interaccion_Y2_MetabTotales.png) | ![Interacción Y3](Graficos/interaccion_Y3_RSD_Todos.png) |

| Y₄ | Y₅ | Y₆ |
|:--:|:--:|:--:|
| ![Interacción Y4](Graficos/interaccion_Y4_RSD_Diana.png) | ![Interacción Y5](Graficos/interaccion_Y5_Indol.png) | ![Interacción Y6](Graficos/interaccion_Y6_Cresol.png) |

| Y₇ | Y₈ | Y₉ |
|:--:|:--:|:--:|
| ![Interacción Y7](Graficos/interaccion_Y7_Metional.png) | ![Interacción Y8](Graficos/interaccion_Y8_AcCarboxilicos.png) | ![Interacción Y9](Graficos/interaccion_Y9_DianasTotales.png) |

**Tabla:** Clasificación cualitativa de cada interacción (fuerte / moderada / débil / nula) con interpretación global por variable.

Cuando las dos líneas del interaction plot son paralelas, la interacción es nula: el efecto de un factor es independiente del nivel del otro. Cuando las líneas se cruzan, la interacción es fuerte y los efectos individuales no pueden interpretarse de forma aislada. Esta exploración visual anticipa cuáles de los términos cruzados β₁₂x₁x₂, β₁₃x₁x₃ y β₂₃x₂x₃ serán significativos en el modelo cuadrático.

---

### 5.5 Correlaciones y residuos preliminares

**Figura 1 — Mapa de calor de correlaciones entre factores:** Muestra la correlación de Pearson entre x₁, x₂ y x₃. En un CCD bien construido los factores codificados son aproximadamente ortogonales (|r| < 0.05), lo que garantiza estimabilidad independiente de cada coeficiente.

![Mapa de calor — correlaciones entre factores](Graficos/correlacion_heatmap.png)

![Correlaciones Y vs Y](Graficos/correlacion_YvsY.png)

**Figura 2 — Modelos lineales preliminares + histogramas de residuos:** Para cada Y se ajusta un modelo lineal puro (sin cuadráticos ni interacciones) y se evalúa la distribución de sus residuos. Un histograma simétrico centrado en cero sugiere que el modelo lineal captura la tendencia central y que los residuos no presentan estructura.

![Histogramas de residuos — modelos lineales preliminares](Graficos/residuos_histogramas.png)

**Figura 3 — Scatter de residuos vs. predichos:** Permite detectar heterocedasticidad (varianza no constante) y estructura en los residuos que el modelo lineal no captura. Un patrón en forma de embudo o de U indica curvatura no modelada, reforzando la necesidad del modelo cuadrático RSM.

![Residuos vs. predichos — modelos lineales](Graficos/residuos_vs_predichos.png)

**Tabla:** Resumen de colinealidad entre factores con interpretación categórica.

---

### 5.6 Modelos preliminares (Fase 3)

**Figura — Comparativa lineal vs. interacciones por variable:** Para cada Y se presentan lado a lado las tablas de coeficientes del modelo lineal (Y = β₀ + β₁x₁ + β₂x₂ + β₃x₃) y del modelo con interacciones de primer orden (añadiendo β₁₂x₁x₂ + β₁₃x₁x₃ + β₂₃x₂x₃), con sus valores t, p-valores y bandas de confianza.

| Y₁ | Y₂ | Y₃ |
|:--:|:--:|:--:|
| ![Modelos Y1](Graficos/fase3_modelos_Y1_MetabDiana.png) | ![Modelos Y2](Graficos/fase3_modelos_Y2_MetabTotales.png) | ![Modelos Y3](Graficos/fase3_modelos_Y3_RSD_Todos.png) |

| Y₄ | Y₅ | Y₆ |
|:--:|:--:|:--:|
| ![Modelos Y4](Graficos/fase3_modelos_Y4_RSD_Diana.png) | ![Modelos Y5](Graficos/fase3_modelos_Y5_Indol.png) | ![Modelos Y6](Graficos/fase3_modelos_Y6_Cresol.png) |

| Y₇ | Y₈ | Y₉ |
|:--:|:--:|:--:|
| ![Modelos Y7](Graficos/fase3_modelos_Y7_Metional.png) | ![Modelos Y8](Graficos/fase3_modelos_Y8_AcCarboxilicos.png) | ![Modelos Y9](Graficos/fase3_modelos_Y9_DianasTotales.png) |

**Tabla comparativa:** R² y R²_ajustado de ambos modelos para las 9 respuestas, junto con los términos significativos (p < 0.05) en cada modelo.

El propósito de esta fase es doble: (i) identificar qué factores tienen efecto lineal claro antes del ajuste cuadrático completo, y (ii) cuantificar el incremento en varianza explicada que aportan las interacciones sobre el modelo lineal puro, como indicador de la complejidad del sistema. Estos modelos son modelos de exploración, **no** el modelo RSM definitivo.

---

### 5.7 Ajuste RSM cuadrático completo (Fase 4)

Esta es la fase central del análisis. El modelo ajustado es:

```
Ŷ = β₀ + β₁x₁ + β₂x₂ + β₃x₃
      + β₁₁x₁² + β₂₂x₂² + β₃₃x₃²
      + β₁₂x₁x₂ + β₁₃x₁x₃ + β₂₃x₂x₃
```

con **n = 17 experimentos**, **p = 10 parámetros** y **7 grados de libertad residuales**.

#### Figura — `Comparacion_RSM.png`

![Comparación RSM — R², F_reg, F_LoF](Graficos/Comparacion_RSM.png)

Panel de tres gráficos de barras horizontales comparativos para las 9 respuestas:
- **R²**: proporción de varianza total explicada por el modelo cuadrático.
- **F_reg**: estadístico F de la regresión completa (contrasta H₀: todos los βᵢ = 0 salvo β₀).
- **F_LoF**: estadístico F del test de Lack-of-Fit (contrasta si el modelo cuadrático es suficiente o si hay curvatura de orden superior no capturada).

Un R² alto (> 0.90) junto con F_reg significativo (p < 0.05) y F_LoF no significativo (p > 0.05) son los tres criterios simultáneos de un buen modelo RSM. F_LoF utiliza las réplicas centrales para separar el error puro del error de modelo, por lo que su interpretación es más exigente que la del F global.

#### Tablas ANOVA por respuesta

Para cada Y se imprime la tabla ANOVA completa con las siguientes fuentes de variación:

| Fuente | g.l. | Suma de cuadrados | Cuadrado medio | F | p-valor |
|--------|------|------------------|----------------|---|---------|
| Regresión | 9 | SS_reg | MS_reg | F_reg | p_reg |
| Residual total | 7 | SS_res | MS_res | — | — |
| Falta de ajuste | 5 | SS_LoF | MS_LoF | F_LoF | p_LoF |
| Error puro | 2 | SS_PE | MS_PE | — | — |
| Total | 16 | SS_tot | — | — | — |

#### Tablas de coeficientes por respuesta

Para cada Y se presenta la tabla de los 10 coeficientes con estimación, error estándar, estadístico t y p-valor bilateral. Los coeficientes cuadráticos (β₁₁, β₂₂, β₃₃) indican curvatura de la superficie; valores negativos implican que la superficie es cóncava hacia abajo (existe un máximo) y valores positivos implican que es convexa (existe un mínimo) en la dirección de ese factor.

#### Tabla maestra — `RSM_Completo_Todos_Y.xlsx`

Exportación a Excel con una hoja por respuesta que contiene: tabla ANOVA, coeficientes con significancias, métricas globales (R², RMSE, PRESS) y los valores predichos vs. observados para los 17 experimentos.

---

### 5.8 Análisis en profundidad de Y₅ — Indol

El Indol (Y₅) recibe un tratamiento diferenciado por ser el único metabolito diana cuya fracción relativa está gobernada por una **interacción sinérgica significativa x₁×x₂** (tiempo de extracción × tiempo de desorción, p = 0.016): el efecto de un tiempo de extracción largo sólo se materializa en una ganancia real de Indol cuando el tiempo de desorción es también elevado.

#### Figura — `Y5_Indol_Superficies.png`

![Y5 Indol — Superficies 3D y contornos](Graficos/Y5_Indol_Superficies.png)

Superficies de respuesta 3D + mapas de contorno para los tres planos de corte (x₁×x₂, x₁×x₃ y x₂×x₃). El tercer factor se fija en su valor óptimo para cada corte. Los mapas de contorno (isolíneas de Ŷ) permiten leer directamente las combinaciones de dos factores que producen el mismo rendimiento y delimitar la región de operación recomendada.

#### Figura — `Y5_Indol_Interaccion.png`

![Y5 Indol — Gráfico de interacción x₁×x₂](Graficos/Y5_Indol_Interaccion.png)

Interaction plot específico de x₁×x₂ para Y₅. El cruce de líneas pone de manifiesto que en el nivel bajo de desorción (x₂ = −1) el tiempo de extracción casi no tiene efecto sobre la fracción de Indol, mientras que en el nivel alto de desorción (x₂ = +1) un mayor tiempo de extracción produce un incremento notable. Este patrón de interacción "disordinal" justifica que la búsqueda del óptimo deba realizarse en la esquina de alta extracción + alta desorción.

---

### 5.9 Interpretación de efectos: Pareto, Superficies, Pred vs Obs (Fase 5)

#### Figuras — `Y1_Pareto.png` … `Y9_Pareto.png`

| Y₁ | Y₂ | Y₃ |
|:--:|:--:|:--:|
| ![Pareto Y1](Graficos/Y1_Pareto.png) | ![Pareto Y2](Graficos/Y2_Pareto.png) | ![Pareto Y3](Graficos/Y3_Pareto.png) |

| Y₄ | Y₅ | Y₆ |
|:--:|:--:|:--:|
| ![Pareto Y4](Graficos/Y4_Pareto.png) | ![Pareto Y5](Graficos/Y5_Pareto.png) | ![Pareto Y6](Graficos/Y6_Pareto.png) |

| Y₇ | Y₈ | Y₉ |
|:--:|:--:|:--:|
| ![Pareto Y7](Graficos/Y7_Pareto.png) | ![Pareto Y8](Graficos/Y8_Pareto.png) | ![Pareto Y9](Graficos/Y9_Pareto.png) |

Diagramas de Pareto de efectos para cada respuesta: barras horizontales ordenadas de mayor a menor por el valor absoluto del estadístico t de cada coeficiente. La línea vertical marca el umbral t crítico (α = 0.05, bilateral). Los términos cuya barra supera la línea son estadísticamente significativos.

El Pareto de efectos es una herramienta de síntesis que comunica de forma inmediata cuáles son los factores dominantes en cada respuesta y si el control sobre esa respuesta proviene de efectos lineales, cuadráticos o de interacción. Permite priorizar los factores en la etapa de optimización y simplificar el modelo si se desea.

#### Figuras — `Y1_Superficies.png` … `Y9_Superficies.png`

| Y₁ | Y₂ | Y₃ |
|:--:|:--:|:--:|
| ![Superficies Y1](Graficos/Y1_Superficies.png) | ![Superficies Y2](Graficos/Y2_Superficies.png) | ![Superficies Y3](Graficos/Y3_Superficies.png) |

| Y₄ | Y₅ | Y₆ |
|:--:|:--:|:--:|
| ![Superficies Y4](Graficos/Y4_Superficies.png) | ![Superficies Y5](Graficos/Y5_Superficies.png) | ![Superficies Y6](Graficos/Y6_Superficies.png) |

| Y₇ | Y₈ | Y₉ |
|:--:|:--:|:--:|
| ![Superficies Y7](Graficos/Y7_Superficies.png) | ![Superficies Y8](Graficos/Y8_Superficies.png) | ![Superficies Y9](Graficos/Y9_Superficies.png) |

Para cada respuesta: panel 2×3 con superficies 3D (fila superior) y mapas de contorno (fila inferior) para los tres planos x₁×x₂, x₁×x₃ y x₂×x₃. El factor ausente se fija en el punto crítico estimado (o en el centro si el punto crítico está fuera de la región experimental).

Las superficies 3D visualizan la geometría de la función de respuesta predicha y permiten identificar visualmente si existe un máximo/mínimo interior o si la respuesta es monótona en el rango estudiado (forma de silla o de rampa). Los mapas de contorno son más útiles para la toma de decisiones operativas porque permiten establecer regiones de trabajo sobre el plano de dos factores.

#### Figura — `PredVsObs_Todas.png`

![Predicho vs Observado — 9 respuestas](Graficos/PredVsObs_Todas.png)

Panel 3×3 con gráficos de dispersión Predicho vs. Observado para las 9 respuestas. Se incluyen la línea de identidad y la banda de ±RMSE. Los puntos próximos a la diagonal confirman que el modelo cuadrático ajusta bien los datos. Puntos alejados de la diagonal son experimentos influyentes o outliers que merecen revisión.

Junto a la figura se genera una **tabla de métricas**: R², RMSE y PRESS (Predicted Residual Error Sum of Squares, estimador del error de predicción cruzada).

---

### 5.10 Localización del óptimo — análisis Hessiano (Fase 6)

#### Figura — `Fase6_Hessianos.png`

![Fase 6 — Puntos críticos y análisis Hessiano](Graficos/Fase6_Hessianos.png)

Para cada respuesta: panel con el punto crítico analítico (solución del sistema ∂Ŷ/∂xᵢ = 0, i = 1, 2, 3), los eigenvalores de la **matriz Hessiana** del modelo cuadrático y la clasificación del punto crítico.

La naturaleza del punto crítico se determina por el signo de los eigenvalores de H = ∂²Ŷ/∂xᵢ∂xⱼ:

| Condición | Clasificación |
|-----------|--------------|
| Todos los eigenvalores < 0 | Máximo |
| Todos los eigenvalores > 0 | Mínimo |
| Eigenvalores de signos mixtos | Punto de silla (saddle) |

El análisis Hessiano es el enfoque analítico clásico en RSM (Myers, Montgomery & Anderson-Cook, 2016). Sus limitaciones son: (i) el punto crítico puede estar fuera de la región experimental, en cuyo caso la predicción es una extrapolación no fiable; (ii) para máximos de respuesta, el punto Hessiano optimiza una sola respuesta a la vez, sin considerar el compromiso entre las 9 variables simultáneamente.

---

### 5.11 Optimización multirrespuesta — deseabilidad (Fase 7)

#### Función de deseabilidad individual

Para cada respuesta Yᵢ se define una función dᵢ ∈ [0, 1] según Derringer & Suich (1980):

- **Maximizar:** dᵢ = [(Yᵢ − L) / (T − L)]^s si L ≤ Yᵢ ≤ T; dᵢ = 1 si Yᵢ ≥ T; dᵢ = 0 si Yᵢ ≤ L
- **Minimizar:** dᵢ = [(U − Yᵢ) / (U − T)]^s si T ≤ Yᵢ ≤ U; dᵢ = 1 si Yᵢ ≤ T; dᵢ = 0 si Yᵢ ≥ U

donde L es el límite inferior aceptable, T el target (óptimo deseado), U el límite superior aceptable y s el exponente de forma (s = 1 implica función lineal).

#### Función de deseabilidad global ponderada

```
D_global = exp [ Σᵢ wᵢ · ln(dᵢ) / Σᵢ wᵢ ]
```

Esta es la **media geométrica ponderada** de las deseabilidades individuales, propuesta por Derringer & Suich (1980) y ampliamente utilizada en química analítica y diseño de procesos. Los pesos utilizados son:

| Respuesta | Peso (w) | Justificación |
|-----------|---------|--------------|
| Y₁ (Metabolitos diana) | 0.20 | Criterio primario de éxito analítico |
| Y₂ (Metabolitos totales) | 0.15 | Cobertura global del método |
| Y₃ (%RSD todos) | 0.10 | Precisión global |
| Y₄ (%RSD diana) | 0.10 | Precisión sobre metabolitos diana |
| Y₅ (Indol) | 0.15 | Metabolito diana volátil de interés |
| Y₆ (Cresol) | 0.10 | Metabolito diana de interés |
| Y₇ (Metional) | 0.10 | Metabolito diana de interés |
| Y₈ (Ác. carboxílicos) | 0.05 | Clase complementaria |
| Y₉ (Dianas/Totales) | 0.05 | Selectividad global |

#### Figura — `Fase7_Deseabilidades.png`

![Fase 7 — Deseabilidades individuales en el óptimo](Graficos/Fase7_Deseabilidades.png)

Panel con las 9 funciones dᵢ(Yᵢ) en el óptimo global: barras que muestran el valor de deseabilidad individual alcanzado por cada respuesta en las condiciones óptimas. Permite visualizar qué respuestas quedan más lejos de su target y cuáles ya alcanzan dᵢ ≈ 1.

#### Figura — `Fase7_Optimo_Global.png`

![Fase 7 — Localización del óptimo global multirrespuesta](Graficos/Fase7_Optimo_Global.png)

Visualización del óptimo global encontrado por optimización numérica (evolución diferencial + L-BFGS-B, Storn & Price 1997; Byrd et al. 1995) en el espacio de factores codificados. Se representa la proyección del óptimo sobre los tres planos de corte (x₁×x₂, x₁×x₃, x₂×x₃) sobreimpuesta sobre el mapa de contorno de D_global.

El óptimo se localiza mediante `scipy.optimize.differential_evolution` como búsqueda global estocástica (evita mínimos locales) seguida de un refinamiento local L-BFGS-B para precisión. La búsqueda se ejecuta con 50 inicios aleatorios independientes para garantizar la convergencia al óptimo global.

#### Figura — `Fase7_Sensibilidad.png`

![Fase 7 — Análisis de sensibilidad del óptimo](Graficos/Fase7_Sensibilidad.png)

Análisis de sensibilidad del óptimo: se perturban los parámetros L, T y U de cada respuesta en ±15% de su rango, y también los pesos wᵢ, evaluando cómo cambia D_global y las coordenadas del óptimo. Las barras muestran la variación relativa de D_global ante cada perturbación.

Un óptimo robusto es aquel cuya D_global varía poco (< 5%) ante perturbaciones razonables en las especificaciones. Si alguna perturbación produce un cambio grande, significa que ese parámetro concreto es crítico y debe establecerse con especial cuidado en la práctica.

#### Tabla — `Resultados_RSM_Fases567.xlsx`

Exportación a Excel con tres hojas: (1) Pareto de efectos para las 9 respuestas, (2) Puntos críticos y eigenvalores Hessianos, (3) Óptimo multirrespuesta con D_global, coordenadas óptimas, valores predichos y deseabilidades individuales.

---

### 5.12 Deseabilidad por experimento con límites ampliados

Esta sección aborda un **problema metodológico relevante** detectado en el cálculo estándar de deseabilidad: cuando L = mínimo observado (como es habitual en implementaciones automáticas), cualquier experimento en ese mínimo obtiene dᵢ = 0 de forma determinista, lo que colapsa D_global a cero a través del efecto multiplicativo de la media geométrica, independientemente del rendimiento de las otras respuestas.

**Solución aplicada (Límites ampliados):**
- Para respuestas a maximizar: L = media − 2·σ
- Para respuestas a minimizar: U = media + 2·σ

Esto traslada el "suelo de deseabilidad cero" fuera del rango experimental real, de modo que ningún experimento observado recibe dᵢ = 0 por el simple hecho de ser el peor de la muestra.

#### Figura — `Desirabilidad_por_Experimento.png`

![Deseabilidad por experimento — original vs límites ampliados](Graficos/Desirabilidad_por_Experimento.png)

Panel superior: barras comparativas de D_global para los 17 experimentos calculada con los límites originales (L = mínimo observado) vs. los límites ampliados (L = media − 2σ). El experimento ganador con los límites ampliados es el que se recomienda para validación experimental.

Panel inferior: desglose de las 9 deseabilidades individuales dᵢ para el experimento de mayor D_global, lo que permite identificar qué respuestas están más lejos de su target en ese punto.

#### Tabla — `df_dc` (impresa en notebook)

Tabla de 17 filas con las condiciones reales (t_ext, t_des, t_inc), las 9 deseabilidades individuales corregidas y la D_global corregida por experimento. Esta tabla es la herramienta de decisión directa: el investigador puede seleccionar el experimento óptimo o un experimento de compromiso entre rendimiento aromático y practicidad operativa.

---

## 6. Conclusiones finales

### 6.1 Calidad del ajuste RSM

El modelo cuadrático RSM ofrece un ajuste satisfactorio para la mayoría de las respuestas (R² > 0.85 en Y₁, Y₂, Y₅), lo que indica que el espacio de diseño CCD cubre adecuadamente la región de variación relevante del sistema. Las respuestas relacionadas con la precisión (%RSD, Y₃ e Y₄) presentan mayor variabilidad residual, coherente con la naturaleza intrínsecamente ruidosa de los coeficientes de variación en análisis cromatográfico.

### 6.2 Efectos dominantes

- **x₁ (t extracción)** es el factor con mayor influencia sobre el número de metabolitos detectados (Y₁, Y₂), con un efecto principalmente lineal positivo: tiempos de extracción más largos aumentan la cobertura metabolómica.
- **x₂ (t desorción)** controla de forma crítica la fracción relativa de los compuestos volátiles diana (Y₅, Y₆, Y₇). Su efecto sobre Indol y Cresol es marcadamente no lineal.
- **x₃ (t incubación)** ejerce un efecto secundario sobre la eficiencia de extracción global, modulando principalmente la selectividad (Y₉).
- La **interacción x₁×x₂** es la más relevante del sistema: Indol (Y₅) sólo alcanza fracciones altas cuando la extracción es larga *y* la desorción es intensa, lo que indica un mecanismo de liberación secuencial que requiere ambas condiciones simultáneamente.

### 6.3 Condiciones operativas recomendadas

Priorizando la recuperación de **Indol + Cresol + Metional** (Y₅, Y₆, Y₇) mediante la optimización ponderada multirrespuesta (pesos conjuntos: 0.35), el óptimo global se localiza en la región de:

- **x₁ > 0** (tiempos de extracción por encima del centro, ~ 90–120 min)
- **x₂ > 0** (tiempos de desorción por encima del centro, ~ 2.5–3 min)
- **x₃ ≈ 0** (tiempo de incubación en torno al valor central, ~ 10 min)

Esta combinación refleja que la extracción larga y la desorción enérgica son los motores del rendimiento en compuestos aromáticos volátiles, mientras que la incubación tiene un papel secundario y puede ajustarse por conveniencia operativa.

### 6.4 Robustez del óptimo

El análisis de sensibilidad muestra que D_global es relativamente estable ante perturbaciones del ±15% en los parámetros de especificación, lo que indica que el óptimo no es un artefacto de la elección concreta de L, T y U sino una característica real de la superficie de respuesta del proceso.

### 6.5 Observación sobre Y₆ e Y₇

Durante el análisis de correlaciones se detectó una **correlación perfecta r = 1.000** entre Cresol (Y₆) y Metional (Y₇), lo que significa que ambas variables contienen exactamente la misma información experimental en este conjunto de datos. Se recomienda revisar el origen de esta redundancia (posible error en la hoja de cálculo o en el protocolo de cuantificación) antes de interpretar sus resultados individualmente en publicaciones futuras.

---

## 7. Archivos del repositorio

```
RSM TFG/
│
├── Untitled-1.ipynb              ← Notebook principal (análisis completo)
├── Variables_y.xlsx              ← Respuestas originales Y₁–Y₉ (17 exp.)
├── datos_transformados.xlsx      ← Respuestas transformadas para RSM
├── Diseño Experimental.xlsx      ← Factores reales (t_ext, t_des, t_inc)
│
└── Graficos/
    ├── Comparacion_RSM.png       ← R², F_reg, F_LoF comparativo
    ├── PredVsObs_Todas.png       ← Predicho vs. Observado (9 respuestas)
    ├── Fase6_Hessianos.png       ← Puntos críticos y eigenvalores
    ├── Fase7_Deseabilidades.png  ← Deseabilidades individuales en el óptimo
    ├── Fase7_Optimo_Global.png   ← Localización del óptimo multirrespuesta
    ├── Fase7_Sensibilidad.png    ← Análisis de sensibilidad del óptimo
    ├── Desirabilidad_por_Experimento.png ← D_global por experimento
    ├── Y[1-9]_Pareto.png         ← Pareto de efectos por respuesta
    ├── Y[1-9]_Superficies.png    ← Superficies 3D + contornos por respuesta
    ├── RSM_Completo_Todos_Y.xlsx ← Maestro ANOVA + coeficientes + métricas
    ├── Resumen_Comparativo_RSM.xlsx  ← Tabla comparativa R², F, LoF
    └── Resultados_RSM_Fases567.xlsx  ← Pareto, Hessiano, Óptimo multirresp.
```

---

## 8. Requisitos

```bash
pip install numpy pandas matplotlib scipy statsmodels openpyxl scikit-learn seaborn
```

Se recomienda **Python 3.10+** y **NumPy ≥ 1.24**. El notebook usa `%matplotlib inline` para renderizado en Jupyter.

Para ejecutar el notebook completo:

```bash
jupyter nbconvert --to notebook --execute Untitled-1.ipynb \
    --output Untitled-1.ipynb --ExecutePreprocessor.timeout=480
```

---

## 9. Referencias bibliográficas

**Diseño de experimentos y RSM:**

- Box, G. E. P., & Behnken, D. W. (1960). Some new three level designs for the study of quantitative variables. *Technometrics*, 2(4), 455–475. https://doi.org/10.1080/00401706.1960.10489912
- Box, G. E. P., & Cox, D. R. (1964). An analysis of transformations. *Journal of the Royal Statistical Society, Series B*, 26(2), 211–243. https://doi.org/10.1111/j.2517-6161.1964.tb00553.x
- Box, G. E. P., & Wilson, K. B. (1951). On the experimental attainment of optimum conditions. *Journal of the Royal Statistical Society, Series B*, 13(1), 1–45.
- Myers, R. H., Montgomery, D. C., & Anderson-Cook, C. M. (2016). *Response Surface Methodology: Process and Product Optimization Using Designed Experiments* (4th ed.). Wiley.
- Montgomery, D. C. (2017). *Design and Analysis of Experiments* (9th ed.). Wiley.

**Optimización multirrespuesta:**

- Derringer, G., & Suich, R. (1980). Simultaneous optimization of several response variables. *Journal of Quality Technology*, 12(4), 214–219. https://doi.org/10.1080/00224065.1980.11980968
- Harrington, E. C. (1965). The desirability function. *Industrial Quality Control*, 21(10), 494–498.

**Tests estadísticos:**

- Shapiro, S. S., & Wilk, M. B. (1965). An analysis of variance test for normality (complete samples). *Biometrika*, 52(3–4), 591–611. https://doi.org/10.2307/2333709
- Cook, R. D. (1977). Detection of influential observation in linear regression. *Technometrics*, 19(1), 15–18. https://doi.org/10.1080/00401706.1977.10489493

**Optimización numérica:**

- Storn, R., & Price, K. (1997). Differential evolution — a simple and efficient heuristic for global optimization over continuous spaces. *Journal of Global Optimization*, 11(4), 341–359. https://doi.org/10.1023/A:1008202821328
- Byrd, R. H., Lu, P., Nocedal, J., & Zhu, C. (1995). A limited memory algorithm for bound constrained optimization. *SIAM Journal on Scientific Computing*, 16(5), 1190–1208. https://doi.org/10.1137/0916069

**Software:**

- Harris, C. R., et al. (2020). Array programming with NumPy. *Nature*, 585, 357–362. https://doi.org/10.1038/s41586-020-2649-2
- Seabold, S., & Perktold, J. (2010). Statsmodels: Econometric and statistical modeling with Python. *Proceedings of the 9th Python in Science Conference*, 57–61.
- Virtanen, P., et al. (2020). SciPy 1.0: Fundamental algorithms for scientific computing in Python. *Nature Methods*, 17, 261–272. https://doi.org/10.1038/s41592-019-0686-2
- Hunter, J. D. (2007). Matplotlib: A 2D graphics environment. *Computing in Science & Engineering*, 9(3), 90–95. https://doi.org/10.1109/MCSE.2007.55

---

*Análisis realizado con Python 3.x · Jupyter Notebook · statsmodels · scipy · numpy · pandas · matplotlib*
