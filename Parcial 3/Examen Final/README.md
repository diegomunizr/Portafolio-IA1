# Examen Final - Parcial 3

## Información de la Actividad

| Campo | Detalle |
|-------|---------|
| **Materia / Curso** | Inteligencia Artificial |
| **Actividad** | Examen Final – Parcial 3 |
| **Tipo de archivo** | PDF con respuestas a preguntas de análisis |
| **Estado** | ✅ Completado |

---

## 📌 Descripción

Este examen final del Parcial 3 aborda tres problemas fundamentales en Inteligencia Artificial y Ciencia de Datos:

1. **Diseño de una estrategia para determinar el número óptimo y la estabilidad de clusters** en un dataset no etiquetado.
2. **Análisis crítico del trabajo de un equipo** que aplicó PCA y regresión logística a un dataset médico, identificando errores, omisiones y áreas de mejora.
3. **Interpretación de clusters** obtenidos a partir de un dataset de enfermedades cardíacas, incluyendo estadísticos descriptivos, pruebas de significancia y hallazgos relevantes.

El examen evalúa la capacidad de:
- Diseñar metodologías robustas para problemas de clustering
- Identificar buenas y malas prácticas en pipelines de machine learning
- Interpretar resultados estadísticos y clínicos a partir de clusters

---

## 📚 Temas Cubiertos

### Pregunta 1 – Estrategia para Clustering
- Preprocesamiento: estandarización (z-score) y PCA
- Determinación del número óptimo de clusters:
  - Elbow method (inercia)
  - Silhouette score
  - Dendrograma de clustering jerárquico
- Aplicación de múltiples métodos de clustering:
  - K-Means (k = 4 y k óptimo)
  - Clustering Jerárquico Aglomerativo (ward, complete, average)
  - DBSCAN (density-based)
- Evaluación de estabilidad con Adjusted Rand Index (ARI)
- Visualización con PCA y conclusiones sobre robustez de los grupos

### Pregunta 2 – Análisis Crítico de Trabajo de Equipo
- Identificación de errores metodológicos:
  - Falta de estandarización previa a PCA
  - Selección arbitraria del número de componentes
  - Uso incorrecto de índices (error de indexación)
  - Interpretación engañosa del accuracy en datos desbalanceados
  - Malinterpretación de loadings de PCA como "variables importantes"
- Elementos que sobran en el flujo de trabajo
- Elementos que faltan:
  - Validación cruzada (k-fold)
  - Manejo de desbalance de clases (SMOTE, class_weight)
  - Comparación con otros clasificadores
  - Reproducibilidad (semilla global, documentación)

### Pregunta 3 – Interpretación de Clusters (Enfermedad Cardíaca)
- Cálculo de estadísticos descriptivos por cluster (medias y proporciones)
- Pruebas de significancia estadística:
  - Mann-Whitney U para variables continuas
  - Chi-cuadrado de Pearson para variables categóricas
- Identificación de hallazgos relevantes:
  - Colesterol sérico (p < 0.001)
  - Vasos coloreados por fluoroscopia (p = 0.003)
  - Vasos con estrechamiento ≥ 50% (p = 0.021)
  - Hipertrofia ventricular en ECG (p = 0.023)
- Caracterización de los clusters:
  - **Cluster 1:** Mayor riesgo cardiovascular (hiperlipidemia severa, más vasos comprometidos)
  - **Cluster 0:** Menor riesgo cardiovascular (perfil lipídico y compromiso arterial moderados)

---

## 🛠️ Herramientas y Métodos Utilizados

- Python (pandas, numpy, scipy)
- Estandarización (z-score)
- PCA (Análisis de Componentes Principales)
- K-Means Clustering
- Clustering Jerárquico Aglomerativo
- DBSCAN
- Adjusted Rand Index (ARI)
- Silhouette Score
- Dendrogramas
- Validación cruzada (k-fold)
- SMOTE / class_weight para desbalance
- Mann-Whitney U test
- Chi-cuadrado de Pearson

---

## 📊 Datos Utilizados

### Pregunta 1 (Dataset hipotético)
- Dataset no etiquetado con estructura de grupos por determinar

### Pregunta 2 (Dataset de imágenes médicas)
- **Pacientes:** 1,000
- **Características:** 500 numéricas por paciente
- **Clases:** 2 (80% control, 20% enfermedad)
- **Uso:** Clasificación con PCA + Regresión Logística

### Pregunta 3 (Dataset de enfermedad cardíaca)
- **Pacientes:** 297
- **Clusters:** 2 (Cluster 0 y Cluster 1)
- **Variables continuas:** edad, presión arterial en reposo, colesterol sérico, frecuencia cardíaca máxima, depresión ST, vasos coloreados, vasos con estrechamiento ≥50%
- **Variables binarias:** sexo, azúcar en ayuno > 120 mg/dL, angina inducida por ejercicio
- **Variables categóricas:** tipo de dolor (cp), ECG en reposo (restecg), pendiente ST (slope), resultado Thallium (thal)

---

## 📈 Hallazgos Clave

### Pregunta 3 – Resultados Estadísticos

| Variable | Cluster 0 | Cluster 1 | p-valor | Significancia |
|----------|-----------|-----------|---------|----------------|
| Colesterol sérico (mg/dL) | 214.26 | 283.96 | < 0.001 | *** |
| Vasos coloreados | 0.53 | 0.84 | 0.0033 | ** |
| Vasos con estrechamiento ≥50% | 0.82 | 1.09 | 0.0209 | * |
| Hipertrofia ventricular (restecg) | 42% | 57% | 0.0229 | * |

**Leyenda:** *** p<0.001, ** p<0.01, * p<0.05, ns = no significativo

**Conclusión principal:** El Cluster 1 agrupa pacientes con **mayor riesgo cardiovascular**, caracterizado por hiperlipidemia severa, más vasos comprometidos y mayor prevalencia de hipertrofia ventricular.

---

## 📁 Documentos Disponibles

| Archivo | Descripción | Enlace de Descarga |
|---------|-------------|---------------------|
| `P3 621784.pdf` | Examen final completo (preguntas y respuestas) | [📥 Descargar PDF](P3%20621784.pdf) |

---

## 👤 Autor

**Diego Emilio Muñiz Ramirez**  
Matrícula: 621784

---

## 🔙 Navegación

- [⬅️ Volver a Parcial 3](../)
- [🏠 Volver al repositorio principal](../../)
