# Examen Parcial 2 – Inteligencia Artificial

**Alumno:** Diego Emilio Muñiz Ramírez  
**Matrícula:** 621784  
**Curso:** SC3314 – Inteligencia Artificial  
**Profesor:** Dr. Antonio Martínez Torteya  
**Universidad de Monterrey**  
**Fecha:** 23 de marzo de 2026  

> *"Doy mi palabra de que he realizado esta actividad con integridad académica"*

---

## 📄 Archivo

| Archivo | Enlace |
|---------|--------|
| `P2 621784.pdf` | [📄 Ver Examen en PDF](https://docs.google.com/viewer?url=https://raw.githubusercontent.com/diegomunizr/Portafolio-IA1/main/Parcial2/Examen%20Parcial%202/P2%20621784.pdf) |

---

## 📋 Contenido del Examen

### Pregunta 1 – Redes Neuronales (MLP)

**a) Conexiones no densas en la primera y última capa**  
Las conexiones de entrada y salida son 1 a 1 — cada flecha va directo a una sola neurona sin pesos entrenables, a diferencia de una capa densa donde cada neurona se conecta con todas las de la siguiente capa.

**b) Conteo de parámetros por capa**

| Capa | Pesos | Sesgos | Total |
|------|-------|--------|-------|
| Entrada(4) → Oculta1(5) | 4×5 = 20 | 5 | 25 |
| Oculta1(5) → Oculta2(5) | 5×5 = 25 | 5 | 30 |
| Oculta2(5) → Salida(4) | 5×4 = 20 | 4 | 24 |
| **Total** | | | **79** |

**c) ¿Está diseñada para imágenes?**  
No. Solo tiene 4 entradas (las imágenes tienen miles de píxeles) y es un MLP sin capas convolucionales. Está diseñada para datos tabulares con 4 variables de entrada.

---

### Pregunta 3 – Algoritmo de Ensamble (Pseudocódigo)

**a) ¿A qué algoritmo se parece?**  
Es una mezcla entre **AdaBoost** y **Bagging**:
- Iteraciones impares → muestrea con pesos y los actualiza enfatizando puntos mal clasificados (AdaBoost)
- Iteraciones pares → muestrea con reemplazo ignorando pesos (Bagging)
- Combinación final mediante la mediana (típico de métodos de ensamble)

**b) ¿Clasificación, regresión o ambos?**  
Tal como está, sirve para **regresión**, ya que la predicción final es la mediana (valor continuo). Para adaptarlo a **clasificación** habría que cambiar la mediana por votación mayoritaria (moda).

---

### Pregunta 4 – Árboles de Decisión y Sobreajuste

**¿Por qué generan sobreajuste sin restricciones?**  
Sin límites, el árbol hace divisiones hasta clasificar perfectamente cada punto de entrenamiento, memorizando los datos en lugar de aprender un patrón general. Aprende el ruido, no el patrón real, por lo que falla con datos nuevos.

**Restricciones observadas en la gráfica mostrada:**
1. `max_depth = 3` — se observan máximo 3 niveles de divisiones anidadas
2. `min_samples_leaf = 5` — las regiones contienen varios puntos, no uno solo

---

### Pregunta 5 – Linear Discriminant Analysis (LDA)

**Dataset:** 150 observaciones, 40 variables de entrada, 3 clases de salida.

**a) ¿Los colores de densidad corresponden con los de dispersión?**  
Sí. Cada clase se proyecta sobre LD1 generando una distribución. La curva verde corresponde a los puntos verdes, la naranja a los naranjas y la azul a los azules. Ambas gráficas muestran lo mismo: una como distribución y otra como puntos en 2D.

**b) ¿LD1 o LD2 tienen mayor poder predictivo?**  
**LD1**, ya que LDA construye los discriminantes ordenados de mayor a menor separación entre clases. En la gráfica de densidad las 3 clases están claramente separadas sobre LD1.

**c) ¿Pueden existir LD3 y LD4?**  
No. El número máximo de discriminantes es min(p, K−1) = min(40, 2) = **2**. Con 3 clases solo pueden existir LD1 y LD2 matemáticamente.

---

### Pregunta 6 – Regresión Logística y Regularización Lasso

**Escenario:** Predicción de diabetes (sí/no) con 150 variables clínicas y 100 pacientes.

**a) Interpretación de coeficientes**

| Variable | Coeficiente | Interpretación |
|----------|-------------|----------------|
| Nivel de glucosa (cuantitativa) | 2.3 | Por cada unidad que sube la glucosa, los odds de tener diabetes se multiplican por e^2.3 ≈ **10** |
| Antecedentes familiares (cualitativa) | 1.6 | Tener antecedentes multiplica los odds por e^1.6 ≈ **5** comparado con no tenerlos |

**b) ¿Sería útil Lasso?**  
Sí, porque hay más variables (150) que observaciones (100), situación propensa al sobreajuste. Lasso penaliza y manda a cero los coeficientes de variables irrelevantes, haciendo selección automática de variables.

**Función a optimizar (Lasso en regresión logística):**

$$\min_{\beta} \left[ -\frac{1}{n} \sum_{i=1}^{n} \left[ y_i \log(\hat{p}_i) + (1 - y_i)\log(1 - \hat{p}_i) \right] + \lambda \sum_{j=1}^{p} |\beta_j| \right]$$

Donde el primer término es la log-verosimilitud negativa (equivalente al RSS en regresión logística) y el segundo es la penalización L1 de Lasso. Lambda controla la intensidad de la penalización.

---

## 🔙 Navegación

- [⬅️ Volver al README del Parcial 2](../README.md)
