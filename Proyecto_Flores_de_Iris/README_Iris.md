# 🌸 Clasificación de Flores Iris — Supervisado vs. No Supervisado

![Python](https://img.shields.io/badge/Python-3.8%2B-blue?logo=python&logoColor=white)
![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-orange?logo=jupyter&logoColor=white)
![Scikit-learn](https://img.shields.io/badge/Scikit--learn-ML-red?logo=scikit-learn&logoColor=white)
![License](https://img.shields.io/badge/Licencia-MIT-green)

Proyecto de Machine Learning que combina clasificación supervisada (SVM) y clustering no supervisado (K-Means) para predecir la especie de flores iris. Sirve como análisis comparativo entre ambos paradigmas de aprendizaje automático.

---

## 📌 Índice

- [Contexto](#-contexto)
- [Objetivo](#-objetivo)
- [Dataset](#-dataset)
- [Metodología](#-metodología)
- [Resultados clave](#-resultados-clave)
- [Tech Stack](#️-tech-stack)
- [Instalación y uso](#-instalación-y-uso)
- [Estructura del proyecto](#-estructura-del-proyecto)
- [Autor](#-autor)
- [Licencia](#-licencia)

---

## 📌 Contexto

El dataset Iris es un clásico en aprendizaje automático, ampliamente utilizado para validar y comparar algoritmos de clasificación y clustering. Su estructura sencilla pero con solapamiento parcial entre clases lo convierte en un banco de pruebas ideal para entender el comportamiento de distintos enfoques de ML.

---

## 🎯 Objetivo

Construir y comparar modelos de clasificación supervisada y no supervisada para predecir la especie de una flor iris, analizando las ventajas e implicaciones de cada enfoque.

---

## 📊 Dataset

Dataset clásico **Iris** (disponible en `scikit-learn`):

| Variable | Descripción |
|---|---|
| `sepal length` | Longitud del sépalo (cm) |
| `sepal width` | Anchura del sépalo (cm) |
| `petal length` | Longitud del pétalo (cm) |
| `petal width` | Anchura del pétalo (cm) |
| `species` *(target)* | Setosa / Versicolor / Virginica |

- **150 registros** · **3 clases balanceadas** · **Sin valores nulos**

---

## 🔧 Metodología

### 1. Análisis Exploratorio (EDA)
- Identificación de patrones y relaciones entre variables.
- Visualización de la separabilidad entre especies.
- Detección del solapamiento entre Versicolor y Virginica.

### 2. Modelado supervisado — SVM
- Entrenamiento de un clasificador SVM con kernel lineal.
- Evaluación con matriz de confusión, classification report y validación cruzada.

### 3. Modelado no supervisado — K-Means
- Clustering con K=3, con y sin escalado previo.
- Evaluación mediante Adjusted Rand Index (ARI) y Silhouette Score.

### 4. Comparación de enfoques
Análisis de los resultados obtenidos por ambos modelos y reflexión sobre cuándo aplicar cada paradigma.

---

## 📈 Resultados clave

### Modelo supervisado — SVM (kernel lineal)

| Métrica | Valor |
|---|---|
| Precisión en test | **100%** |
| Validación cruzada | **98%** (±1.63%) |

```
Matriz de confusión:
[[19  0  0]
 [ 0 13  0]
 [ 0  0 13]]
```

| Clase | Precision | Recall | F1-Score |
|---|---|---|---|
| Setosa | 1.00 | 1.00 | 1.00 |
| Versicolor | 1.00 | 1.00 | 1.00 |
| Virginica | 1.00 | 1.00 | 1.00 |

### Modelo no supervisado — K-Means

| Configuración | ARI | Silhouette Score |
|---|---|---|
| Sin escalado | **0.72** | **0.55** |
| Con escalado | 0.43 | 0.48 |

> El escalado empeoró los resultados, indicando que las variables originales ya se encuentran en una escala adecuada para este dataset.

### Insights clave
- **Iris Setosa** es completamente separable del resto por sus características de pétalo.
- Existe solapamiento entre **Versicolor y Virginica**, lo que introduce mayor complejidad.
- Los modelos **supervisados superan claramente** a los no supervisados en este problema.
- El preprocesamiento (como el escalado) puede tener un efecto negativo si no es necesario.

---

## 🛠️ Tech Stack

| Librería | Uso |
|---|---|
| `pandas` / `numpy` | Manipulación de datos |
| `scikit-learn` | Modelos, métricas y datasets |
| `matplotlib` / `seaborn` | Visualización |
| `jupyter` | Entorno interactivo |

---

## 🚀 Instalación y uso

### Requisitos previos
- Python 3.8 o superior
- `pip`

### Pasos

```bash
# 1. Clona el repositorio
git clone https://github.com/christianirshool-glitch/My-projects.git
cd My-projects/Proyecto_Flores_de_Iris

# 2. Crea y activa un entorno virtual (recomendado)
python -m venv venv
source venv/bin/activate       # Linux/macOS
venv\Scripts\activate          # Windows

# 3. Instala las dependencias
pip install pandas numpy scikit-learn matplotlib seaborn jupyter

# 4. Lanza el notebook
jupyter notebook iris_classification.ipynb
```

> El dataset Iris se carga directamente desde `scikit-learn`, por lo que no es necesario descargar ningún archivo adicional.

---

## 📁 Estructura del proyecto

```
Proyecto_Flores_de_Iris/
├── iris_classification.ipynb    # Notebook principal
└── README.md
```

---

## 👤 Autor

**Christian Méndez Giraldo**  
Data Scientist · MSc in Data Science  
[GitHub](https://github.com/christianirshool-glitch)

---

## 📄 Licencia

Este proyecto está bajo la licencia **MIT** — consulta el archivo [LICENSE](../LICENSE) para más detalles.
