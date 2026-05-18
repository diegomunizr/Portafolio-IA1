# A3.3 Multiple Testing y Aprendizaje por Refuerzo

## Información de la Actividad

| Campo | Detalle |
|-------|---------|
| **Materia / Curso** | Inteligencia Artificial |
| **Actividad** | A3.3 – Tutorial de Multiple Testing y Aprendizaje por Refuerzo |
| **Tipo de archivo** | Jupyter Notebook (.ipynb) y PDF |
| **Estado** | ✅ Completado |

---

## 📌 Descripción

En esta actividad se abordaron dos temas fundamentales en Inteligencia Artificial: **Multiple Testing (Pruebas Múltiples)** y **Aprendizaje por Refuerzo (Reinforcement Learning)**.

La primera parte se centra en el problema de la **inflación del error Tipo I** al realizar múltiples pruebas estadísticas simultáneas y cómo corregirlo mediante métodos como Bonferroni, Holm y Benjamini-Hochberg (FDR).

La segunda parte introduce el **aprendizaje por refuerzo** utilizando el entorno CartPole. Se implementó un agente **Q-Learning** que aprende a equilibrar un poste mediante interacción con el entorno, utilizando una estrategia epsilon-greedy y discretización del espacio de estados.

---

## 📚 Temas Cubiertos

### 🧪 Multiple Testing
- El problema de las pruebas múltiples e inflación del error Tipo I
- Family-Wise Error Rate (FWER)
- Corrección de Bonferroni
- Corrección de Holm (Bonferroni secuencial)
- False Discovery Rate (FDR)
- Método de Benjamini-Hochberg (BH)
- Aplicación a un dataset de expresión génica (6,830 genes)
- Visualización y comparación de métodos de corrección

### 🎮 Aprendizaje por Refuerzo (Reinforcement Learning)
- Conceptos fundamentales: agente, entorno, estado, acción, recompensa, política
- Entorno CartPole-v1
- Algoritmo Q-Learning
- Ecuación de actualización de Bellman
- Estrategia epsilon-greedy (exploración vs explotación)
- Discretización del espacio de estados continuos
- Implementación completa del agente
- Entrenamiento, curva de aprendizaje y evaluación

---

## 🛠️ Herramientas Utilizadas

- Python 3
- Jupyter Notebook
- NumPy
- Pandas
- Matplotlib
- SciPy (pruebas estadísticas)
- statsmodels (correcciones de pruebas múltiples)
- Gymnasium (entorno CartPole)
- PyVirtualDisplay (renderizado en entornos sin pantalla)

---

## 📁 Documentos Disponibles

| Archivo | Descripción | Enlace de Descarga |
|---------|-------------|---------------------|
| `A3.3 621784.ipynb` | Desarrollo completo de la actividad en Jupyter Notebook | [📥 Descargar Notebook](A3.3%20621784.ipynb) |
| `A3.3 621784.pdf` | Exportación en PDF del notebook | [📥 Descargar PDF](A3.3%20621784.pdf) |

---

## 📊 Conjuntos de Datos y Entornos Utilizados

### 1. Simulación de Juego de Monedas
- **Participantes:** 1,000
- **Lanzamientos por participante:** 10
- **Probabilidad de águila:** 0.5
- **Uso:** Demostración del problema de pruebas múltiples

### 2. Dataset de Expresión Génica (Sintético)
- **Genes:** 6,830
- **Muestras:** 60 (30 control + 30 tratamiento)
- **Genes verdaderamente diferentes:** 5%
- **Uso:** Aplicación de correcciones de pruebas múltiples en un escenario realista

### 3. Entorno CartPole-v1 (Gymnasium)
- **Descripción:** Poste unido a un carro que debe mantenerse en equilibrio
- **Estado (observación):** 4 variables continuas (posición, velocidad, ángulo, velocidad angular)
- **Acciones:** 2 (empujar izquierda/derecha)
- **Objetivo:** Maximizar pasos con el poste erguido (máximo 500)
- **Uso:** Entrenamiento de agente Q-Learning

---

## 📁 Documentos Disponibles

| Archivo | Descripción | Enlace |
|---------|-------------|--------|
| `A3.3 621784.ipynb` | Desarrollo completo de la actividad en Jupyter Notebook | [📓 Ver Notebook](./A3.3%20621784.ipynb) |
| `A3.3 621784.pdf` | Exportación en PDF del notebook | [📄 Ver PDF](./A3.3%20621784.pdf) |

---

## ✅ Resultados

### Multiple Testing
- Se demostró la inflación del error Tipo I en pruebas múltiples (6830 pruebas → ~341 falsos positivos esperados por azar)
- **Bonferroni:** Control estricto del FWER, pero muy conservador
- **Holm:** Mismo control que Bonferroni con mayor poder estadístico
- **FDR BH (1%):** Mayor poder, ideal para exploración de datos genómicos

### Aprendizaje por Refuerzo
- Se implementó un agente Q-Learning con discretización del espacio de estados
- El agente aprendió a equilibrar el poste mediante exploración epsilon-greedy
- La curva de aprendizaje mostró mejora progresiva en la recompensa acumulada
- El agente entrenado logró mantener el equilibrio por períodos significativos

Actividad completada satisfactoriamente.

---

## 🔙 Navegación

- [⬅️ Volver a Parcial 3](../)
- [🏠 Volver al repositorio principal](../../)
