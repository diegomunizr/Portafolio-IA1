# A2.3 Modelos de Ensamble, SVM y Redes Neuronales

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

- 📄 [Ver Reporte en PDF](https://docs.google.com/viewer?url=https://raw.githubusercontent.com/diegomunizr/Portafolio-IA1/main/Parcial2/A2.3%20Modelos%20de%20ensamble%2C%20SVM%20y%20refes%20neuronales/A2.3%20621784.pdf)
- 📓 [Descargar Notebook](https://github.com/diegomunizr/Portafolio-IA1/blob/main/Parcial2/A2.3%20Modelos%20de%20ensamble%2C%20SVM%20y%20refes%20neuronales/A2.3%20621784.ipynb?raw=true)
- 🗃️ [Descargar Dataset](https://github.com/diegomunizr/Portafolio-IA1/blob/main/Parcial2/A2.3%20Modelos%20de%20ensamble%2C%20SVM%20y%20refes%20neuronales/wine_dataset.csv?raw=true)

---

## 📌 Descripción

Entrenamiento y comparación de cuatro modelos avanzados de clasificación multiclase sobre el **Wine Dataset**: Random Forest, Gradient Boosting, SVM y Red Neuronal (MLP). Se analiza el desempeño, complejidad e interpretabilidad de cada enfoque.

---

## 📊 Dataset

Archivo: `wine_dataset.csv`

| Característica | Detalle |
|----------------|---------|
| Muestras | 178 |
| Variables predictoras | 13 (alcohol, ácido málico, ceniza, alcalinidad, magnesio, fenoles totales, flavanoides, fenoles no flavanoides, proantocianinas, intensidad de color, tonalidad, OD280/OD315, prolina) |
| Variable de salida | Variedad de vino (3 clases) |
| Valores nulos | 0 |
| Partición | 80% entrenamiento (142) / 20% prueba (36), estratificada |

### Distribución de clases

| Clase | Total | Entrenamiento | Prueba |
|-------|-------|---------------|--------|
| class_0 | 59 | 47 | 12 |
| class_1 | 71 | 57 | 14 |
| class_2 | 48 | 38 | 10 |

---

## 🤖 Modelos y configuración

### 1. Random Forest

| Hiperparámetro | Valor | Justificación |
|----------------|-------|---------------|
| `n_estimators` | 100 | Buen balance entre varianza y costo computacional |
| `max_depth` | None | Sin límite; el ensamble controla el sobreajuste |
| `random_state` | 42 | Reproducibilidad |

### 2. Gradient Boosting

| Hiperparámetro | Valor | Justificación |
|----------------|-------|---------------|
| `n_estimators` | 100 | 100 etapas de boosting |
| `learning_rate` | 0.1 | Tasa moderada |
| `max_depth` | 3 | Árboles débiles para evitar sobreajuste individual |

### 3. SVM (kernel RBF)

| Hiperparámetro | Valor | Justificación |
|----------------|-------|---------------|
| `C` | 1.0 | Penalización estándar; equilibra margen y errores |
| `kernel` | `'rbf'` | Apropiado para datos con estructura no lineal |
| `gamma` | `'scale'` | Ajuste automático basado en la varianza de las features |

> SVM y Red Neuronal requieren escalado previo con `StandardScaler`.

### 4. Red Neuronal (MLP)

| Hiperparámetro | Valor | Justificación |
|----------------|-------|---------------|
| `hidden_layer_sizes` | (64, 32) | Arquitectura piramidal |
| `activation` | `'relu'` | Evita el problema del gradiente desvaneciente |
| `max_iter` | 500 | Suficientes iteraciones para convergencia |

---

## 📈 Resultados

### Comparativa de métricas (conjunto de prueba, n=36)

| Modelo | Accuracy | Precision (macro) | Recall (macro) | F1-Score (macro) |
|--------|----------|-------------------|----------------|------------------|
| **Random Forest** | **1.0000** | **1.0000** | **1.0000** | **1.0000** |
| SVM | 0.9722 | 0.9778 | 0.9667 | 0.9710 |
| Red Neuronal | 0.9722 | 0.9778 | 0.9667 | 0.9710 |
| Gradient Boosting | 0.9444 | 0.9505 | 0.9429 | 0.9453 |

### Errores por modelo

- **Random Forest**: 0 errores — clasificación perfecta.
- **SVM y Red Neuronal**: 1 error cada uno — una muestra de class_2 clasificada como class_1.
- **Gradient Boosting**: 2 errores — confusiones entre class_1 y class_2.

> Todos los errores se concentran entre class_1 y class_2, sugiriendo que estas dos variedades tienen características fisicoquímicas más similares entre sí que con class_0.

### Variables más importantes (Random Forest)

Las variables más discriminativas según importancia Gini son `color_intensity`, `flavanoids` y `proline`. Las variables `ash` y `alcalinity_of_ash` contribuyen mínimamente.

---

## 🔍 Análisis crítico

**¿El aumento de complejidad se tradujo en mejoras claras?**
No. En este dataset pequeño (178 muestras), Random Forest alcanzó clasificación perfecta sin necesidad de modelos más complejos. Esto es consistente con la teoría: en datasets pequeños y bien estructurados, modelos más simples suelen ser suficientes y más estables.

**Riesgo de sobreajuste:** La perfección de Random Forest en prueba (1.0000) sobre 178 muestras merece cautela. Una evaluación con k-fold cross-validation (k=5 o k=10) ofrecería una estimación más robusta.

### Interpretabilidad relativa

| Modelo | Interpretabilidad |
|--------|-------------------|
| Random Forest | Alta — importancia de variables y árboles visualizables |
| Gradient Boosting | Moderada — importancia disponible, proceso secuencial difícil de rastrear |
| SVM | Baja — decisión basada en vectores de soporte en espacio kernel RBF |
| Red Neuronal | La más baja — los pesos no tienen interpretación directa |

### ¿Cuándo preferir cada modelo?

- **Random Forest**: Punto de partida ideal. Robusto, rápido e interpretable.
- **Gradient Boosting**: Preferible con datasets grandes, ruidosos o con relaciones complejas.
- **SVM**: Útil en espacios de alta dimensión con clases separables.
- **Red Neuronal**: Justificada con alto volumen de datos y relaciones altamente no lineales.

---

## 💡 Conclusión

Para el Wine Dataset, el **Random Forest representa el mejor balance** entre desempeño, interpretabilidad y simplicidad. La elección del modelo siempre depende del contexto: volumen de datos, necesidad de interpretabilidad, recursos computacionales y tolerancia al riesgo de sobreajuste.

---

## 🛠️ Herramientas utilizadas

- Python 3
- `pandas`, `numpy`, `matplotlib`, `seaborn`
- `scikit-learn` (RandomForestClassifier, GradientBoostingClassifier, SVC, MLPClassifier, StandardScaler)

---

## 🔙 Navegación

- [⬅️ Volver a Parcial 2](../)
- [🏠 Volver al repositorio principal](../../)
