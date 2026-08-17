# 💳 Detección de Fraude en Tarjetas de Crédito

![Python](https://img.shields.io/badge/Python-3.8%2B-blue?logo=python&logoColor=white)
![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-orange?logo=jupyter&logoColor=white)
![Scikit-learn](https://img.shields.io/badge/Scikit--learn-ML-red?logo=scikit-learn&logoColor=white)
![imbalanced-learn](https://img.shields.io/badge/imbalanced--learn-SMOTE-purple)
[![License](https://img.shields.io/badge/Licencia-MIT-green)](LICENSE)

Proyecto completo de ciencia de datos orientado a la detección de transacciones fraudulentas con tarjeta de crédito. Incluye análisis exploratorio, detección de anomalías con Isolation Forest, tratamiento del desbalanceo de clases (SMOTE y `class_weight`) y modelado predictivo con Random Forest, evaluado con métricas específicas para problemas altamente desbalanceados (ROC AUC, PR-AUC).

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

El fraude con tarjetas de crédito representa una amenaza constante para entidades financieras y usuarios. El principal reto técnico de este problema no es solo predictivo, sino estadístico: las transacciones fraudulentas son extremadamente raras (en este dataset, apenas un **0.17%** del total), lo que exige técnicas específicas de tratamiento de clases desbalanceadas y métricas de evaluación distintas a las habituales (accuracy no es fiable en este escenario).

---

## 🎯 Objetivo

Construir un modelo capaz de identificar transacciones fraudulentas con tarjeta de crédito, priorizando un equilibrio entre **recall** (detectar el mayor número posible de fraudes reales) y **precisión** (minimizar falsas alarmas), evaluado mediante métricas robustas frente al desbalanceo de clases.

---

## 📊 Dataset

El dataset **`creditcard.csv`** (fuente: [Kaggle — mlg-ulb/creditcardfraud](https://www.kaggle.com/mlg-ulb/creditcardfraud)) contiene **284.807 transacciones** realizadas por tarjetahabientes europeos durante dos días de septiembre de 2013, con **31 columnas**:

| Columna | Descripción |
|---|---|
| `Class` | Variable objetivo — 0 = transacción legítima, 1 = fraude |
| `Time` | Segundos transcurridos desde la primera transacción del dataset |
| `Amount` | Importe de la transacción |
| `V1`...`V28` | Componentes numéricos anonimizados obtenidos mediante PCA (por confidencialidad, no se dispone del significado original de las variables) |

**Distribución del target:**

| Clase | Recuento | Porcentaje |
|---|---|---|
| 0 — No fraude | 284.315 | 99.83% |
| 1 — Fraude | 492 | 0.17% |

El dataset no presenta valores nulos.

---

## 🔧 Metodología

### 1. Carga e inspección inicial
Descarga del dataset desde Kaggle (`kagglehub`), revisión de dimensiones, tipos de datos (todas las variables son numéricas) y comprobación de valores nulos (ninguno).

### 2. Análisis exploratorio (EDA)
- **Estadísticas descriptivas** de todas las variables (media, mediana, desviación estándar, percentiles).
- **Detección de outliers univariante (IQR)**: aplicado a las 30 variables numéricas para tener una primera referencia rápida de dispersión.
- **Detección de outliers multivariante (Isolation Forest)**: se prioriza sobre el IQR porque evalúa las variables de forma conjunta (no columna a columna), no asume una distribución concreta y evita el problema de falsos positivos que surge al aplicar IQR a 30 columnas por separado. Con `contamination=0.01`, detecta un 1% de la muestra como outliers.
  - El cruce entre outliers detectados y la clase real mostró que Isolation Forest capturó el **58.7%** de los fraudes reales sin usar la etiqueta durante el entrenamiento, con un **enriquecimiento de fraude ~59x** dentro del grupo de outliers frente a la tasa base. Esto valida la hipótesis de que el fraude tiende a manifestarse como comportamiento anómalo.
  - La variable `is_outlier_IF` (resultado del Isolation Forest) se incorpora como **feature adicional** para el modelo supervisado.
- **Correlación con el target**: cálculo de correlación biserial puntual (*point-biserial*) para variables numéricas frente a `Class`. Las variables con mayor asociación fueron `V17`, `V14`, `V12`, `is_outlier_IF`, `V10`, `V16`, `V3` y `V7`.
- **Visualización de distribuciones**: histogramas por variable segmentados por clase, y scatter plots de las variables más correlacionadas frente al target, con línea de tendencia y coeficiente de correlación.

### 3. Preprocesamiento para modelado
- **División train/test**: 70% entrenamiento / 30% test, con **estratificación** para mantener la proporción de fraude en ambos conjuntos.
- **Escalado**: `RobustScaler`, elegido por su robustez frente a outliers (usa mediana y rango intercuartílico en lugar de media y desviación estándar).

### 4. Tratamiento del desbalanceo de clases
Se exploraron dos estrategias:
- **SMOTE (Synthetic Minority Over-sampling Technique)**: aplicado únicamente sobre el conjunto de entrenamiento ya escalado (nunca sobre test, para evitar data leakage), generando muestras sintéticas de la clase minoritaria hasta equilibrar ambas clases (199.020 vs 199.020).
- **`class_weight='balanced'`**: penaliza más los errores sobre la clase minoritaria durante el entrenamiento del Random Forest, sin necesidad de generar muestras sintéticas.

### 5. Modelado predictivo
- **Random Forest Classifier** (`n_estimators=100`, `max_depth=15`, `min_samples_split=10`, `min_samples_leaf=5`, `class_weight='balanced'`).
- Entrenado sobre los datos escalados con `RobustScaler`, usando las 31 variables disponibles (`V1`-`V28`, `Time`, `Amount`, `is_outlier_IF`).

### 6. Evaluación
Dado el fuerte desbalanceo, el **accuracy** no es una métrica fiable (un modelo que prediga siempre "no fraude" ya obtendría ~99.8%). Se priorizan:
- **Precision / Recall** de la clase fraude.
- **ROC AUC**.
- **PR-AUC (Precision-Recall AUC)**, considerada la métrica principal por su sensibilidad a los falsos positivos en escenarios de alta rareza de la clase positiva.
- **Matriz de confusión**, con especial atención a los **falsos negativos** (fraudes no detectados), el error más costoso en este dominio.

### 7. Función de predicción
Se implementó `predict_fraud()`, que recibe una transacción nueva, la escala con el mismo `scaler` ajustado en entrenamiento, y devuelve la probabilidad de fraude, la predicción binaria y un nivel de riesgo (`LOW` / `MEDIUM` / `HIGH`) según el umbral de probabilidad.

---

## 📈 Resultados clave

Modelo final: **Random Forest con `class_weight='balanced'`**, evaluado sobre el conjunto de test (85.443 transacciones, 148 fraudes reales).

| Métrica | Valor |
|---|---|
| Accuracy | 99.9% |
| Precision (fraude) | 86.9% |
| Recall / Sensibilidad (fraude) | 76.4% |
| F1-Score | 81.3% |
| ROC AUC | 0.952 |
| **PR-AUC** | **0.808** |

**Matriz de confusión:**

| | Predicho: No Fraude | Predicho: Fraude |
|---|---|---|
| **Real: No Fraude** | 85.278 (TN) | 17 (FP) |
| **Real: Fraude** | 35 (FN) | 113 (TP) |

- El modelo detecta correctamente el **76.4%** de los fraudes reales, con una precisión del **86.9%** en las transacciones marcadas como fraude.
- El **PR-AUC de 0.808** confirma un buen equilibrio entre capturar fraude real y limitar falsas alarmas, siendo la métrica más representativa dado el desbalanceo extremo del dataset.
- Las **35 transacciones fraudulentas no detectadas** (falsos negativos) representan el margen de mejora más crítico del modelo.
- Las variables `V17`, `V14`, `V12`, `V10`, `V16`, `V3` y `V7`, junto con la señal de anomalía `is_outlier_IF`, son las de mayor asociación con el fraude.

---

## 🛠️ Tech Stack

| Librería | Uso |
|---|---|
| `pandas` | Manipulación de datos |
| `numpy` | Operaciones numéricas |
| `matplotlib` / `seaborn` | Visualización |
| `scikit-learn` | Preprocesamiento, Isolation Forest, Random Forest, métricas |
| `imbalanced-learn` | SMOTE para balanceo de clases |
| `scipy` | Tests estadísticos (point-biserial, chi-cuadrado) |
| `kagglehub` | Descarga del dataset desde Kaggle |
| `jupyter` | Entorno interactivo |

---

## 🚀 Instalación y uso

### Requisitos previos
* Todas las dependencias están listadas en el archivo [requirements.txt](requirements.txt).
* Cuenta de Kaggle configurada (para la descarga automática vía `kagglehub`), o descarga manual del dataset [creditcard.csv](https://www.kaggle.com/mlg-ulb/creditcardfraud).

### Pasos de ejecución

```bash
# 1. Clona el repositorio
git clone https://github.com/christianirshool-glitch/My-projects.git
cd My-projects

# 2. Crea y activa un entorno virtual (recomendado)
python -m venv venv
source venv/bin/activate       # En Linux/macOS
venv\Scripts\activate          # En Windows

# 3. Instala las dependencias
pip install -r requirements.txt

# 4. Lanza el entorno interactivo
jupyter notebook "Project_1_Credit_Fraud_Detection.ipynb"
```

> 📌 El notebook descarga el dataset automáticamente mediante `kagglehub.dataset_download("mlg-ulb/creditcardfraud")`. Si no dispones de credenciales de Kaggle configuradas, descarga manualmente `creditcard.csv` y ajusta la ruta de carga en el notebook.

---

## 📁 Estructura del proyecto

```
credit-fraud-detection/
├── Project_1_Credit_Fraud_Detection.ipynb   # Notebook principal con todo el pipeline
├── requirements.txt                          # Dependencias del proyecto
├── LICENSE                                    # Licencia de uso MIT
└── README.md                                  # Documentación del proyecto
```

---

## 👤 Autor

**Christian Méndez Giraldo**
Data Scientist · MSc in Data Science
[GitHub](https://github.com/christianirshool-glitch)

---

## 📄 Licencia

Este proyecto está bajo la Licencia MIT - mira el archivo [LICENSE](LICENSE) para más detalles.
