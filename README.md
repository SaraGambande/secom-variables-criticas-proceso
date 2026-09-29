# Variables críticas de proceso en fabricación de semiconductores (SECOM)

Análisis estadístico de datos reales de una línea de fabricación de semiconductores. El objetivo es identificar qué variables de proceso están asociadas a las fallas de calidad y evaluar qué tan confiable es esa conclusión.

## Datos
- **Fuente:** [SECOM Dataset — UCI Machine Learning Repository](https://archive.ics.uci.edu/dataset/179/secom)
- **Contenido:** 1.567 lotes y 590 variables de sensores y puntos de control, registrados entre julio y octubre de 2008.
- **Resultado:** cada lote tiene una etiqueta de pasa/falla en el control de calidad final. La tasa de fallas es del 6,6%.

## Metodología
| Etapa | Técnica |
|---|---|
| Depuración y estandarización | Eliminación de variables constantes o con >50% de faltantes, imputación por mediana, corrección de formato de fechas |
| Tendencia de desempeño | Tasa de fallas semanal, prueba chi-cuadrado |
| Variables críticas | Pruebas de hipótesis Mann-Whitney U con corrección Benjamini-Hochberg (FDR) y tamaño de efecto |
| Redundancia | Correlación de Spearman |
| Estructura del proceso | Análisis de componentes principales (PCA) |
| Cuantificación de efectos | Regresión logística (Statsmodels), odds ratios |
| Validación | Validación cruzada estratificada y comparación contra baseline (ROC-AUC, balanced accuracy) |

## Resultados principales
- **Calidad de datos:** 144 de las 590 variables no eran utilizables (constantes o con demasiados faltantes).
- **Tendencia:** la tasa de fallas bajó de 8,6% a 4,7% entre la primera y la segunda mitad del período (p ≈ 0,003).
- **Variables críticas:** de 446 variables útiles, 25 difieren significativamente entre lotes buenos y fallados después de corregir por comparaciones múltiples. Sin corregir eran 86.
- **Menos es más:** un modelo con las 11 variables críticas no redundantes (ROC-AUC ≈ 0,72) supera a uno con las 446 variables (≈ 0,66).
- **Limitación:** son datos observacionales, así que muestran asociación y no causalidad. Como siguiente paso se propone un **Diseño de Experimentos (DoE)**: un factorial fraccionado sobre las variables críticas controlables.

## Estructura
```
├── secom_analisis_variables_criticas.ipynb   # análisis completo, ya ejecutado
├── data/uci-secom.csv                        # dataset
├── powerbi/                                  # tablas exportadas para dashboard
│   ├── tasa_fallas_semanal.csv
│   ├── variables_criticas.csv
│   └── lotes_variables_criticas.csv
└── requirements.txt
```

## Cómo ejecutarlo
```bash
pip install -r requirements.txt
jupyter notebook secom_analisis_variables_criticas.ipynb
```

## Herramientas
Python · Pandas · NumPy · SciPy · Statsmodels · scikit-learn · Matplotlib · Seaborn · Power BI
