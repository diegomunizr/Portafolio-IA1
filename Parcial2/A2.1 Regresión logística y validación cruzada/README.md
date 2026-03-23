# A2.1 Regresión Logística y Validación Cruzada

## Información

| Campo | Detalle |
|-------|---------|
| **Alumno** | Diego Emilio Muñiz Ramírez |
| **Matrícula** | 621784 |
| **Fecha** | 24/02/2026 |
| **Archivo** | `A2_1_Regresión_logística_y_validación_cruzada.ipynb` |

## Descripción

Implementación de un modelo de **regresión logística** para clasificación binaria de propiedades inmobiliarias (departamento vs. casa), aplicando **validación cruzada estratificada** y análisis de métricas de rendimiento.

## Dataset

Archivo: `dataset.csv`

Base de datos de propiedades inmobiliarias. La variable de salida es `is_apartment` (1 = departamento, 0 = casa). Se eliminaron observaciones de tipo `store` y `PH` por ser minorías fuera de la clasificación binaria de interés.

### Variables utilizadas (features)

| Variable | Descripción |
|----------|-------------|
| `price_usd_per_m2` | Precio en USD por metro cuadrado |
| `surface_total_in_m2` | Superficie total en m² |
| `surface_covered_in_m2` | Superficie cubierta en m² |
| `price_per_m2` | Precio por metro cuadrado |
| `price_aprox_usd` | Precio total aproximado en USD |

## Metodología

### 1. Importación y preparación de datos
Carga del dataset, filtrado de clases relevantes y preparación de variables de entrada y salida.

### 2. Separación entrenamiento / prueba (80/20)
Se usó `stratify=y` en `train_test_split` para garantizar que la proporción de clases sea similar en entrenamiento y prueba. Las características se estandarizaron con `StandardScaler`.

### 3. Validación cruzada
Se aplicó **Stratified K-Fold** con `k=10` sobre los datos de entrenamiento para estimar la exactitud del modelo sin tocar el conjunto de prueba.

### 4. Entrenamiento final y evaluación con distintos umbrales
Se entrenó el modelo sobre todos los datos de entrenamiento y se evaluó la exactitud, sensibilidad y especificidad para tres umbrales: **0.3, 0.5 y 0.7**.

### 5. Curva ROC y AUC
Se graficó la curva ROC para ilustrar el trade-off entre sensibilidad y especificidad en todos los umbrales posibles.

### 6. Interpretación de coeficientes
Los coeficientes del modelo (sobre variables estandarizadas) representan el cambio en el **log-odds** de que una propiedad sea departamento.

## Resultados

| Métrica | Valor |
|---------|-------|
| Exactitud (validación cruzada) | ~76% |
| AUC (curva ROC) | ~0.81 |

### Coeficientes del modelo

| Característica | Coeficiente | Interpretación |
|----------------|-------------|----------------|
| `price_usd_per_m2` | **+2.75** | Mayor precio por m² → mayor probabilidad de ser departamento |
| `surface_total_in_m2` | **+1.37** | Positivo moderado: propiedades con mayor superficie total |
| `surface_covered_in_m2` | **−1.34** | Mayor superficie cubierta reduce la probabilidad de ser departamento |
| `price_per_m2` | **−0.65** | Negativo leve, captura diferencias de escala entre monedas |
| `price_aprox_usd` | **−1.73** | Precio total alto en USD → tiende a ser casa |

## Conclusión

El predictor más influyente es el precio por metro cuadrado en USD (+2.75): los departamentos se concentran en zonas de alta densidad urbana con mayor valor por metro. En sentido contrario, un precio total alto en USD reduce la probabilidad de ser departamento, pues las casas tienden a ser más grandes y costosas en términos absolutos. El modelo alcanza una exactitud de ~76% en validación cruzada y un AUC de ~0.81, lo que indica un buen poder discriminativo.

## Herramientas utilizadas

- Python 3
- `pandas`, `numpy`
- `scikit-learn` (LogisticRegression, StratifiedKFold, cross_val_score, StandardScaler)
- `matplotlib`
