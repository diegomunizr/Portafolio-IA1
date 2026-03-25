# A2.2 LDA y Árboles de Decisión

## Información

| Campo | Detalle |
|-------|---------|
| **Alumno** | Diego Emilio Muñiz Ramírez |
| **Matrícula** | 621784 |
| **Curso** | SC3314 – Inteligencia Artificial |
| **Profesor** | Dr. Antonio Torteya |
| **Fecha** | 24/02/2026 |

---

## 📂 Archivos del proyecto

- 📄 [Ver Reporte en PDF](https://docs.google.com/viewer?url=https://raw.githubusercontent.com/diegomunizr/Portafolio-IA1/main/Parcial2/A2.2%20LDA%20y%20%C3%A1rboles%20de%20decisi%C3%B3n/A2.2%20621784.pdf)
- 📓 [Descargar Notebook](https://github.com/diegomunizr/Portafolio-IA1/blob/main/Parcial2/A2.2%20LDA%20y%20%C3%A1rboles%20de%20decisi%C3%B3n/A2.2%20621784.ipynb?raw=true)

---

## 📌 Descripción

Comparación de dos enfoques de clasificación multiclase: **Linear Discriminant Analysis (LDA)** y **Árboles de Decisión**, aplicados al dataset Wine de scikit-learn. Se construyen, visualizan y comparan ambos modelos evaluando su desempeño con métricas estándar.

---

## 📊 Dataset

**Wine** (UCI Machine Learning Repository, incluido en scikit-learn).

| Característica | Detalle |
|----------------|---------|
| Muestras | 178 |
| Variables predictoras | 13 (numéricas continuas: alcohol, ácido málico, cenizas, magnesio, flavonoides, color, etc.) |
| Variable de salida | Cultivar del vino (3 clases) |
| Partición | 70% entrenamiento (124) / 30% prueba (54), estratificada |

### Distribución de clases

| Clase | Muestras | Porcentaje |
|-------|----------|------------|
| class_0 | 59 | 33.1% |
| class_1 | 71 | 39.9% |
| class_2 | 48 | 27.0% |

---

## 🧪 Metodología

### Modelo 1 — LDA

- Se utilizaron las 13 variables continuas como predictores.
- LDA genera 2 funciones discriminantes lineales (LD1 y LD2) que maximizan la varianza entre clases.
- **LD1** explica el **66.69%** de la varianza entre clases; **LD2** explica el **33.31%** restante (100% total).
- Se visualizó la proyección bidimensional y las fronteras de decisión lineales.

### Modelo 2 — Árbol de Decisión

- Criterio de impureza: **Gini**.
- Árbol sin podar: profundidad 4, 8 hojas, accuracy en test 96.30%.
- Poda mediante **Cost-Complexity Pruning** con α óptimo seleccionado por validación cruzada de 5 pliegues.
- α óptimo: **0.01541** → Árbol podado: profundidad 3, 5 hojas.

### Reglas del árbol podado
```
color_intensity ≤ 3.82                          → class_1
color_intensity > 3.82 y flavanoids ≤ 1.58
    alcalinity_of_ash ≤ 17.65                   → class_1
    alcalinity_of_ash > 17.65                   → class_2
color_intensity > 3.82 y flavanoids > 1.58
    proline ≤ 724.5                              → class_1
    proline > 724.5                              → class_0
```

### Variables más importantes (árbol podado)

| Variable | Importancia (Gini) |
|----------|--------------------|
| `flavanoids` | 0.454 |
| `color_intensity` | 0.429 |
| `proline` | 0.110 |
| `alcalinity_of_ash` | 0.006 |

---

## 📈 Resultados

### Comparativa de métricas (conjunto de prueba, n=54)

| Métrica | LDA | Árbol de Decisión |
|---------|-----|-------------------|
| **Accuracy** | **0.9815** | 0.9444 |
| Precision (macro) | **0.9825** | 0.9583 |
| Recall (macro) | **0.9841** | 0.9407 |
| F1-Score (macro) | **0.9829** | 0.9467 |
| Errores totales | **1** | 3 |

### LDA — Reporte por clase

| Clase | Precision | Recall | F1 |
|-------|-----------|--------|----|
| class_0 | 0.95 | 1.00 | 0.97 |
| class_1 | 1.00 | 0.95 | 0.98 |
| class_2 | 1.00 | 1.00 | 1.00 |

### Árbol de Decisión — Reporte por clase

| Clase | Precision | Recall | F1 |
|-------|-----------|--------|----|
| class_0 | 1.00 | 0.89 | 0.94 |
| class_1 | 0.88 | 1.00 | 0.93 |
| class_2 | 1.00 | 0.93 | 0.97 |

---

## 💡 Conclusión

LDA resultó el modelo más adecuado para este problema por tres razones: los supuestos de normalidad multivariada y homocedasticidad se cumplen razonablemente en datos de análisis químico controlado; la proyección bidimensional resume el 100% de la variabilidad entre clases; y comete solo 1 error frente a los 3 del árbol.

El Árbol de Decisión, aunque ligeramente inferior en accuracy (94.44%), ofrece una ventaja práctica importante: sus reglas if-else son directamente interpretables por un experto sin conocimiento estadístico, siendo valioso en contextos industriales o clínicos.

**Conclusión general:** cuando los supuestos paramétricos se satisfacen, LDA es preferible por su solidez estadística y reducción dimensional. El árbol es la alternativa cuando la interpretabilidad es prioritaria o los supuestos de normalidad no se cumplen.

---

## 📚 Referencias

- Fisher, R. A. (1936). *The use of multiple measurements in taxonomic problems.* Annals of Eugenics, 7(2), 179–188.
- Bishop, C. M. (2006). *Pattern Recognition and Machine Learning.* Springer.
- Breiman, L. et al. (1984). *Classification and Regression Trees.* CRC Press.
- Pedregosa, F. et al. (2011). Scikit-learn: Machine Learning in Python. *JMLR*, 12, 2825–2830.
- UCI Machine Learning Repository. (1991). Wine Data Set.

---

## 🛠️ Herramientas utilizadas

- Python 3
- `pandas`, `numpy`, `matplotlib`, `seaborn`
- `scikit-learn` (LinearDiscriminantAnalysis, DecisionTreeClassifier, cross_val_score, ConfusionMatrixDisplay)

---

## 🔙 Navegación

- [⬅️ Volver a Parcial 2](../)
- [🏠 Volver al repositorio principal](../../)
