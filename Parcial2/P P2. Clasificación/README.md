# P P2 – Clasificación de Vinos mediante Análisis Químico

**Curso:** SC3314 – Inteligencia Artificial  
**Universidad:** Universidad de Monterrey  
**Profesor:** Dr. Antonio Martínez Torteya  
**Alumno:** Diego Emilio Muñiz Ramírez  

---

## Descripción del Proyecto

Este proyecto implementa un sistema de clasificación multiclase para identificar la variedad de uva de origen de un vino a partir de su perfil químico. Se utilizan algoritmos de aprendizaje automático entrenados sobre el clásico **Wine Dataset** del UCI Machine Learning Repository (Forina et al., 1991), que contiene análisis químicos de vinos italianos provenientes de tres variedades distintas de uva.

El problema se enmarca como una tarea de **clasificación supervisada**, dado que la variable objetivo (`tipo_vino`) es categórica, discreta y sin relación ordinal entre clases.

---

## Archivos del Proyecto

| Archivo | Descripción | Descarga |
|---------|-------------|----------|
| `P_P2_621784.ipynb` | Notebook principal con el análisis completo | [⬇ Descargar Notebook](./P_P2_621784.ipynb) |
| `wine_dataset.csv` | Dataset de vinos (UCI ML Repository) | [⬇ Descargar Dataset](./wine_dataset.csv) |
| `P_P2_621784.pdf` | Reporte en PDF del proyecto | [⬇ Descargar PDF](./P_P2_621784.pdf) |

---

## Dataset

| Propiedad | Valor |
|-----------|-------|
| Fuente | UCI Machine Learning Repository |
| Referencia | Forina et al. (1991) |
| Filas | 178 |
| Columnas | 15 (13 predictoras + 1 objetivo + 1 nombre) |
| Valores nulos | 0 |

### Variable Objetivo

`tipo_vino` — Variable categórica con tres clases:

- **Clase 0** – Variedad A (59 muestras, 33.1%)
- **Clase 1** – Variedad B (71 muestras, 39.9%)
- **Clase 2** – Variedad C (48 muestras, 27.0%)

### Variables Predictoras

| Variable | Descripción | Unidad |
|----------|-------------|--------|
| `alcohol` | Contenido alcohólico | % vol. |
| `malic_acid` | Concentración de ácido málico | g/L |
| `ash` | Contenido de cenizas | g/L |
| `alcalinity_of_ash` | Alcalinidad de las cenizas | mEq/L |
| `magnesium` | Concentración de magnesio | mg/L |
| `total_phenols` | Contenido total de fenoles | mg/L |
| `flavanoids` | Concentración de flavonoides | mg/L |
| `nonflavanoid_phenols` | Fenoles no flavonoides | mg/L |
| `proanthocyanins` | Concentración de proantocianinas | mg/L |
| `color_intensity` | Intensidad del color | Abs. |
| `hue` | Tonalidad del color | Ratio |
| `od280/od315_of_diluted_wines` | Ratio de densidad óptica | Ratio |
| `proline` | Concentración del aminoácido prolina | mg/L |

---

## Metodología

### Preparación de Datos

- Verificación de valores nulos (ninguno encontrado)
- División train/test estratificada: **80% / 20%** (142 / 36 muestras)
- Estandarización con `StandardScaler` ajustado **solo** sobre el conjunto de entrenamiento (evita data leakage)

### Modelos Entrenados

Se entrenaron y compararon 6 modelos mediante **validación cruzada de 5 folds**:

| Modelo | Accuracy (CV) | F1-W (CV) | Acc Std |
|--------|--------------|-----------|---------|
| LDA | 0.9929 | 0.9929 | 0.0143 |
| SVM (RBF) | 0.9862 | 0.9863 | 0.0276 |
| Regresión Logística | 0.9860 | 0.9861 | 0.0172 |
| Random Forest | 0.9862 | 0.9860 | 0.0276 |
| Gradient Boosting | 0.9372 | 0.9369 | 0.0513 |
| Red Neuronal (MLP) | 0.8032 | 0.7703 | 0.1200 |

### Modelo Final Seleccionado

**Random Forest** (`n_estimators=200`, `random_state=42`)

Criterios de selección:
- Métricas muy altas en las tres dimensiones evaluadas
- Robusto ante multicolinealidad entre predictores
- Permite calcular importancia de variables (interpretabilidad práctica)
- Baja varianza entre folds (modelo estable)

---

## Resultados en Conjunto de Prueba

| Métrica | Valor |
|---------|-------|
| Accuracy | **1.0000 (100%)** |
| F1-score (weighted) | **1.0000** |
| AUC-ROC (OvR) | **1.0000** |

El modelo clasificó correctamente las 36 muestras del conjunto de prueba, con precisión, recall y F1-score de 1.00 para cada una de las tres variedades.

### Variables Más Importantes

1. `color_intensity` — 0.1874
2. `flavanoids` — 0.1684
3. `proline` — 0.1570
4. `alcohol` — 0.1082
5. `hue` — 0.0950

---

## Requisitos

```bash
pip install pandas numpy matplotlib seaborn scikit-learn
```

### Dependencias principales

- Python 3.8+
- pandas
- numpy
- matplotlib
- seaborn
- scikit-learn

---

## Cómo Ejecutar

1. Clona o descarga el repositorio.
2. Asegúrate de que `wine_dataset.csv` esté en el mismo directorio que el notebook.
3. Abre `P_P2_621784.ipynb` en Jupyter Notebook, JupyterLab o Google Colab.
4. Ejecuta todas las celdas en orden (`Runtime > Run all` en Colab).

---

## Conclusiones

- La composición química de un vino es **altamente informativa** respecto a su variedad de origen.
- No es necesario analizar los 13 compuestos para una clasificación confiable; un subconjunto reducido de variables clave podría ser suficiente en producción.
- Random Forest equilibra mejor la precisión con la utilidad práctica frente a otros modelos evaluados.
- El modelo tiene potencial de integrarse en sistemas de **control de calidad** o **certificación de origen** en bodegas y plantas embotelladoras.

### Limitaciones

- Dataset pequeño (178 muestras) — poder estadístico limitado.
- No incluye factores como año de cosecha, condiciones climáticas o proceso de vinificación.
- Desbalance moderado entre clases que podría afectar escenarios reales.

### Trabajo Futuro

- Recolectar datos de vinos mexicanos (Valle de Guadalupe, Querétaro).
- Optimización de hiperparámetros mediante búsqueda bayesiana.
- Explorar reducción de dimensionalidad con 4-5 variables clave.
- Desplegar el modelo como API REST para uso en bodegas en tiempo real.

---

## Referencias

Forina, M. et al. (1991). *PARVUS: An Extendable Package for Data Exploration, Classification and Correlation*. Institute of Pharmaceutical and Food Analysis and Technologies. UCI Machine Learning Repository – Wine Dataset.

---

## 🔙 Navegación

- [⬅️ Volver al README del Parcial 2](../README.md)
