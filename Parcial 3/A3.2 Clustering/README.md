# A3.2 Clustering

## Información de la Actividad

| Campo | Detalle |
|-------|---------|
| **Materia / Curso** | Inteligencia Artificial |
| **Actividad** | A3.2 – Tutorial de Clustering |
| **Tipo de archivo** | Jupyter Notebook (.ipynb) y PDF |
| **Estado** | ✅ Completado |

---

## 📌 Descripción

En esta actividad se trabajó con técnicas de **clustering (aprendizaje no supervisado)** utilizando dos métodos clásicos: **K-Means** y **Clustering Jerárquico**. A diferencia del aprendizaje supervisado, aquí no se utilizan etiquetas previas. El objetivo es descubrir estructuras ocultas en los datos agrupando observaciones similares.

El desarrollo incluyó:
- Generación y visualización de un dataset sintético
- Implementación paso a paso del algoritmo K-Means
- Aplicación de K-Means al dataset real NCI60 (expresión génica de líneas celulares de cáncer)
- Uso de PCA para reducción de dimensionalidad y visualización
- Implementación de Clustering Jerárquico con diferentes métodos de enlace
- Análisis e interpretación de dendrogramas

---

## 📚 Temas Cubiertos

### 📊 K-Means Clustering
- Introducción al algoritmo K-Means
- Pasos del algoritmo: inicialización, asignación, actualización y convergencia
- Visualización del proceso paso a paso con dataset sintético
- Aplicación al dataset NCI60
- Uso de PCA para visualización de clusters en alta dimensionalidad

### 🌲 Clustering Jerárquico
- Introducción al clustering jerárquico aglomerativo
- Métodos de enlace: complete, average, single, ward
- Construcción e interpretación de dendrogramas
- Comparación con resultados de K-Means

### 📉 PCA (Principal Component Analysis)
- Reducción de dimensionalidad para visualización
- Transformación de 6,830 genes a 2-3 componentes principales
- Visualización de clusters en espacio PCA

---

## 🛠️ Herramientas Utilizadas

- Python 3
- Jupyter Notebook
- NumPy
- Pandas
- Matplotlib
- Scikit-learn (KMeans, PCA, AgglomerativeClustering, make_blobs)
- SciPy (dendrogramas)
- ISLP (carga del dataset NCI60)

---

## 📁 Documentos Disponibles

| Archivo | Descripción | Enlace de Descarga |
|---------|-------------|---------------------|
| `A3.2 621784.ipynb` | Desarrollo completo de la actividad en Jupyter Notebook | [📥 Descargar Notebook](A3.2%20621784.ipynb) |
| `A3.2 621784.pdf` | Exportación en PDF del notebook | [📥 Descargar PDF](A3.2%20621784.pdf) |

---

## 📊 Conjuntos de Datos Utilizados

### 1. Dataset Sintético (`make_blobs`)
- **Muestras:** 1,000
- **Características:** 2 (para facilitar la visualización en 2D)
- **Clusters reales:** 3
- **Uso:** Demostración paso a paso del algoritmo K-Means

### 2. Dataset NCI60
- **Descripción:** Datos de expresión génica del Instituto Nacional del Cáncer de EE. UU.
- **Muestras:** 64 líneas celulares de cáncer
- **Genes (características):** 6,830
- **Tipos de cáncer:** 14 (pulmón, mama, colon, CNS, melanoma, etc.)
- **Uso:** Benchmark para evaluar clustering en datos de alta dimensión

---

## 🖼️ Evidencias

| Archivo | Descripción |
|---------|-------------|
| `A3.2 621784.ipynb` | Desarrollo completo de la actividad en Jupyter Notebook |
| `A3.2 621784.pdf` | Exportación en PDF de la actividad terminada |

---

## ✅ Resultados

- **K-Means:** Implementación exitosa, mostrando la evolución de los clusters desde una asignación aleatoria hasta la convergencia.
- **Clustering Jerárquico:** Construcción de dendrogramas que permiten visualizar la jerarquía de agrupaciones.
- **NCI60:** Ambos métodos lograron agrupar líneas celulares por tipo de cáncer, demostrando su utilidad en bioinformática.
- **PCA:** Reducción dimensional efectiva que permitió visualizar clusters en 2D y 3D.
- Actividad completada satisfactoriamente.

---

## 🔙 Navegación

- [⬅️ Volver a Parcial 3](../)
- [🏠 Volver al repositorio principal](../../)
