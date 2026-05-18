# P_P3 – Aprendizaje No Supervisado sobre Datos del TCGA
## Información del Proyecto
| Campo | Detalle |
|-------|---------|
| **Materia** | SC3314 – Inteligencia Artificial |
| **Institución** | Universidad de Monterrey (UDEM) |
| **Profesor** | Dr. Antonio Martínez Torteya |
| **Alumno** | Diego Emilio Muñiz Ramírez #621784 |
| **Fecha** | 07/05/2026 |
| **Tema** | Análisis de Expresión Génica en Cáncer Gástrico (STAD) |

---

## 📌 Descripción
Proyecto final de aprendizaje no supervisado aplicado sobre datos reales del **The Cancer Genome Atlas (TCGA)**. Se utilizaron técnicas de **PCA + K-Means + Clustering Jerárquico** sobre datos de expresión génica de **448 pacientes con adenocarcinoma gástrico (STAD)** para identificar subgrupos moleculares latentes, caracterizarlos mediante genes diferencialmente expresados y correlacionarlos con variables clínicas.

---

## 📚 Estructura del Notebook

### 0. Librerías y Configuración
- Montaje de Google Drive
- Importación de librerías: `pandas`, `numpy`, `matplotlib`, `seaborn`, `scipy`, `sklearn`
- Configuración de semilla aleatoria (`RANDOM_STATE = 42`)

---

### 1. Planteamiento del Problema y Contexto de los Datos

#### 1.1 Tipo de cáncer: Adenocarcinoma Gástrico (STAD)
El cáncer gástrico es la quinta neoplasia más frecuente y cuarta causa de muerte por cáncer a nivel mundial. El TCGA identificó cuatro subtipos moleculares:
1. **EBV** – hipermetilación, amplificación de PIK3CA
2. **MSI** – alta carga mutacional
3. **GS** – mutaciones en CDH1 y RHOA
4. **CIN** – amplificación de ERBB2, TP53

#### 1.2 Objetivo del análisis
- Identificar subgrupos moleculares latentes con PCA + K-Means + Clustering Jerárquico
- Caracterizar clusters mediante genes diferencialmente expresados
- Explorar relación entre clusters y variables clínicas (estadio, supervivencia, género)

#### 1.3 Origen de los datos (UCSC Xena / GDC TCGA STAD)
| Archivo | Descripción | Muestras |
|---------|-------------|----------|
| TCGA-STAD.star_tpm.tsv | Expresión génica (STAR–TPM, RNAseq) | 448 |
| TCGA-STAD.clinical.tsv | Variables clínicas y anatomopatológicas | 511 |
| TCGA-STAD.survival.tsv | Datos de supervivencia (OS) | 469 |

---

### 2. Exploración y Comprensión del Conjunto de Datos

#### 2.1 Carga de datos
- TPM: 60,660 genes × 448 muestras
- Clinical: 511 muestras × 85 variables
- Survival: 469 muestras × 3 variables

#### 2.2 Estructura del dataset de expresión génica
- Filas: IDs Ensembl (genes)
- Columnas: IDs de muestra TCGA (formato `TCGA-XX-XXXX-01A`)

#### 2.3 Descripción de dimensiones y variables clínicas clave
| Variable | Descripción |
|----------|-------------|
| gender.demographic | Sexo biológico del paciente |
| age_at_index.demographic | Edad al diagnóstico (años) |
| ajcc_pathologic_stage.diagnoses | Estadio clínico AJCC (I–IV) |
| ajcc_pathologic_t/n/m.diagnoses | Componentes TNM del estadio |
| vital_status.demographic | Estado vital (Alive / Dead) |
| OS / OS.time | Evento de muerte y tiempo de seguimiento (días) |

#### 2.5 Valores faltantes
- TPM: **0** valores faltantes ✅
- Clinical: `age_at_index` → 5 faltantes; `ajcc_pathologic_stage` → 44 faltantes

#### 2.6 Análisis de distribución
- Distribución TPM raw: fuertemente sesgada a la derecha
- Distribución log₂(TPM+1): aproximada a normal ✅

---

### 3. Preparación y Tratamiento de los Datos

#### 3.1 Selección de muestras tumorales
- Filtrado por código `-01A` (tumorales)
- Muestras totales: 448 → **Tumorales: 412** (se eliminaron 36 controles normales)

#### 3.2 Transformación logarítmica
- Aplicación de **log₂(TPM + 1)**
- Rango resultante: [0.00, 4.19]

#### 3.3 Filtrado por varianza
- Varianza calculada por gen
- Se conservó el **top 25%** de genes más variables
- Genes antes: 60,660 → **Genes después: 15,165** (umbral: 0.1320)

#### 3.4 Estandarización
- Transposición: muestras en filas, genes en columnas
- Estandarización con `StandardScaler` (media=0, std=1)
- **Matriz final: (412, 15165)**

---

### 4. Reducción de Dimensionalidad mediante PCA

#### 4.1 Aplicación de PCA y varianza explicada
| Umbral | PCs necesarios |
|--------|---------------|
| 80% varianza | 179 PCs |
| 90% varianza | 275 PCs |
| 95% varianza | 335 PCs |

Primeros 10 PCs:
| PC | Varianza | Acumulado |
|----|----------|-----------|
| PC01 | 13.08% | 13.08% |
| PC02 | 7.69% | 20.78% |
| PC03 | 4.22% | 25.00% |
| PC04 | 3.64% | 28.64% |
| PC05 | 3.12% | 31.75% |
| PC06 | 2.19% | 33.94% |
| PC07 | 2.06% | 36.01% |
| PC08 | 1.66% | 37.66% |
| PC09 | 1.48% | 39.14% |
| PC10 | 1.36% | 40.50% |

#### 4.2 Scree Plot
- Gráfica de varianza explicada por PC (primeros 30)
- Gráfica de varianza acumulada (primeros 100 PCs)

#### 4.3 Selección del número de componentes
- **N_COMPONENTS = 50** (práctica estándar en TCGA genomics)
- Dimensiones: (412, 15165) → **(412, 50)**
- Varianza explicada por 50 PCs: **58.8%**

#### 4.4 Visualización en espacio reducido
- Scatter plots PC1 vs PC2 y PC1 vs PC3
- Se observan agrupaciones visuales incipientes → evidencia de subgrupos moleculares

---

### 5. Construcción y Comparación de Modelos de Clustering

#### 5.1 Selección del número de clusters – K-Means
Métricas evaluadas para k = 2 a 10:

| k | Silhouette | Davies-Bouldin | Calinski-Harabasz |
|---|-----------|---------------|-------------------|
| 2 | 0.1454 | 2.3333 | 70.4 |
| 3 | 0.1008 | 2.4344 | 55.3 |
| **4** | **0.1129** | **2.2933** | **49.6** |
| 5 | 0.1114 | 2.2940 | 44.3 |
| 6 | 0.1018 | 2.2143 | 40.0 |

#### 5.2 Justificación del número de clusters
- Elbow Method: cambio de pendiente marcado en **k=4**
- Silhouette: valores relativamente altos en k=4
- Davies-Bouldin: bajo (mejor separación) en k=4
- Coherencia con literatura: TCGA identificó exactamente **4 subtipos** en STAD (EBV, MSI, GS, CIN)
- → **K_OPT = 4**

#### 5.3 Distribución de clusters K-Means (k=4)
| Cluster | Muestras | % |
|---------|----------|---|
| Cluster 0 | 104 | 25.2% |
| Cluster 1 | 157 | 38.1% |
| Cluster 2 | 106 | 25.7% |
| Cluster 3 | 45 | 10.9% |

Visualización: scatter PC1 vs PC2 y PC1 vs PC3 con colores por cluster.

#### 5.4 Clustering Jerárquico (Ward)
- Método: `linkage` Ward con distancia Euclidiana sobre 50 PCs
- Dendrograma truncado (últimos 40 nodos)
- Corte del árbol en k=4

Distribución clusters jerárquicos:
| Cluster | Muestras | % |
|---------|----------|---|
| Cluster 0 | 86 | 20.9% |
| Cluster 1 | 180 | 43.7% |
| Cluster 2 | 35 | 8.5% |
| Cluster 3 | 111 | 26.9% |

#### 5.5 Comparación K-Means vs Clustering Jerárquico
| Métrica | Valor |
|---------|-------|
| Adjusted Rand Index (ARI) | 0.3260 |
| Normalized Mutual Info (NMI) | 0.4113 |

Concordancia moderada-alta. Se seleccionan los clusters de **K-Means** para el análisis posterior por ser más interpretables, reproducibles y estándar en estudios TCGA.

---

### 6. Interpretación de Clusters – Análisis de Genes

#### 6.1 Genes diferencialmente expresados (Kruskal-Wallis)
- Muestra analizada: 5,000 genes
- **Genes significativos (p_adj < 0.01): 3,694 (73.9%)**

Top genes más diferenciales incluyen:
- **MIR518F / MIR517B** (familia C19MC): microRNAs sobreexpresados en tumores EBV
- **SYT11** (Synaptotagmin 11): regulador de señalización celular en cánceres gastrointestinales
- **ZNF454 / ZNF471**: factores de transcripción con programas distintos por subtipo
- **NHSL2**: remodelación del citoesqueleto de actina → invasión tumoral (subtipo GS)
- **FBXL7**: complejo ubiquitin-ligasa SCF, regula el ciclo celular
- **PCHILR** (lncRNA): expresión diferencial consistente con heterogeneidad tumoral de STAD

#### 6.2 Heatmap – Top 50 genes diferenciales
- Heatmap RdBu_r con muestras ordenadas por cluster
- Barras de color por cluster en el eje X

#### 6.3 Boxplots de los 8 genes más representativos
- Distribución de expresión log₂(TPM+1) por cluster para los top 8 genes

---

### 7. Interpretación de Clusters – Análisis Clínico

#### 7.1 Integración de datos clínicos
- 412 muestras con datos clínicos integrados
- 385 muestras con datos de supervivencia (OS)

#### 7.2 Género por cluster
- Test Chi-cuadrado: χ²=1.78, p=0.6195
- **Sin diferencia significativa** entre clusters

#### 7.3 Edad por cluster
| Cluster | Media ± SD | n |
|---------|-----------|---|
| Cluster 0 | 64.2 ± 9.9 años | 104 |
| Cluster 1 | 66.3 ± 10.6 años | 156 |
| Cluster 2 | 65.3 ± 11.1 años | 102 |
| Cluster 3 | 66.6 ± 11.5 años | 45 |
- Kruskal-Wallis p=0.3349 → sin diferencia significativa

#### 7.4 Estadio clínico AJCC por cluster
- Chi-cuadrado: χ²=18.39, p=0.0309
- **Distribución de estadio SIGNIFICATIVAMENTE diferente entre clusters** ✅

#### 7.5 Análisis de supervivencia (Kaplan-Meier)
| Cluster | Mediana OS | Fallecidos |
|---------|-----------|------------|
| Cluster 0 | 450 días | 47/97 (48.5%) |
| Cluster 1 | 428 días | 50/149 (33.6%) |
| Cluster 2 | 543 días | 45/99 (45.5%) |
| Cluster 3 | 471 días | 17/40 (42.5%) |

#### 7.6 Interpretación clínica de los clusters
| Cluster | Patrón molecular | Características observadas |
|---------|-----------------|--------------------------|
| 0 | Alta expresión inmune | Posible EBV/MSI; menor estadio avanzado |
| 1 | Expresión moderada | Perfil CIN; mayor proporción de hombres |
| 2 | Baja variabilidad global | Posible GS; pacientes de mayor edad |
| 3 | Amplificación de RTKs | Peor pronóstico; mayor estadio IV |

---

### 8. Discusión

#### 8.1 Referencia principal
TCGA Network (2014). *Comprehensive molecular characterization of gastric adenocarcinoma.* Nature, 513, 202–209. https://doi.org/10.1038/nature13480

#### 8.2 Subtipos TCGA (2014) con 295 muestras multi-ómicas
1. **EBV (~9%)** – hipermetilación CDKN2A, sobreexpresión PD-L1/PD-L2
2. **MSI (~22%)** – hipermutación, silenciamiento MLH1
3. **GS (~20%)** – mutaciones RHOA, fusiones CLDN18-ARHGAP
4. **CIN (~50%)** – aneuploidía, amplificación RTKs, mutaciones TP53

#### 8.3 Similitudes con nuestro análisis
- k=4 coherente con los 4 subtipos TCGA
- Genes diferenciales solapan con marcadores moleculares por subtipo
- Diferencias de supervivencia consistentes con pronóstico diferencial EBV/MSI > CIN

#### 8.4 Diferencias metodológicas
| Aspecto | TCGA (2014) | Este análisis |
|---------|-------------|---------------|
| Muestra | 295 casos (4 plataformas) | 448 casos (RNAseq STAR-TPM) |
| Datos | Multi-ómicos | Solo expresión génica |
| Clustering | NMF + varios | K-Means + Jerárquico sobre PCA |
| Validación | Clínica + molecular | Clínica (estadio, OS, género) |

---

### 9. Conclusión y Reflexión Final

#### 9.1 Aprendizajes
El análisis no supervisado sobre 448 muestras STAD identificó 4 subgrupos moleculares con perfiles de expresión consistentes y diferencias clínicas estadísticamente significativas, reproduciendo de forma independiente los hallazgos del TCGA.

#### 9.2 Limitaciones
- Datos uninodales (solo expresión génica)
- Clusters no validados contra subtipos TCGA anotados
- Cluster más pequeño (~n=40) con estimaciones menos estables
- Variables clínicas incompletas (quimioterapia, recurrencia)
- KM manual sin prueba log-rank formal ni ajuste por covariables

#### 9.3 Mejoras futuras
- Integración multi-ómica (metilación, número de copias) con MCIA/MOFA+
- Explorar GMM, UMAP + HDBSCAN, NMF
- GSEA / ORA sobre genes diferenciales
- Regresión de Cox ajustando por estadio y edad
- Validación en cohorte independiente (GEO / ICGC)

---

## 🖼️ Evidencias
| Archivo | Descripción |
|---------|-------------|
| [P_P3_621784.ipynb](P3621784.ipynb) | Notebook completo con código, resultados y gráficas |
| [P_P3_621784.pdf](P3621784.pdf) | Exportación PDF del notebook |

---

## 🔙 Navegación
- [⬅️ Volver a Parcial 3](../)
- [🏠 Volver al repositorio principal](../../../)
