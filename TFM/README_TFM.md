# 💳 easyMoney — Customer Analytics & Predictive Modeling

![Python](https://img.shields.io/badge/Python-3.8%2B-blue?logo=python&logoColor=white)
![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-orange?logo=jupyter&logoColor=white)
![Power BI](https://img.shields.io/badge/Power%20BI-Dashboard-yellow?logo=powerbi&logoColor=white)
![License](https://img.shields.io/badge/Licencia-MIT-green)

Trabajo de Fin de Máster (MSc Data Science & AI) enfocado en un caso real de negocio: **easyMoney**, una plataforma de productos financieros cuyo objetivo es maximizar la rentabilidad de su base de clientes mediante analítica avanzada y modelos predictivos.

🌐 **Web del proyecto:** [tfm0925grupo6.work](https://www.tfm0925grupo6.work/)

---

## 📌 Índice

- [Contexto](#-contexto)
- [Objetivo](#-objetivo)
- [Dataset](#-dataset)
- [Metodología](#-metodología)
- [Modelos desarrollados](#-modelos-desarrollados)
- [Resultados e insights](#-resultados-e-insights)
- [Dashboard Power BI](#-dashboard-power-bi)
- [Tech Stack](#️-tech-stack)
- [Instalación y uso](#-instalación-y-uso)
- [Estructura del proyecto](#-estructura-del-proyecto)
- [Autor](#-autor)
- [Licencia](#-licencia)

---

## 📌 Contexto

EasyMoney es una fintech en fase de crecimiento que enfrenta tres retos clave:

- Baja rentabilidad por cliente en determinados segmentos.
- Toma de decisiones comerciales basada en intuición y no en datos.
- Falta de herramientas para identificar clientes con alta propensión de compra.

Este proyecto simula el trabajo de un equipo de Data Science en un entorno real, con requerimientos abiertos y fuerte orientación a negocio.

---

## 🎯 Objetivo

Desarrollar soluciones analíticas que permitan a easyMoney:

- Entender el comportamiento y perfil de sus clientes.
- Identificar segmentos con características y necesidades diferenciadas.
- Predecir la propensión de compra de nuevos productos financieros.
- Facilitar la toma de decisiones comerciales mediante visualización interactiva.

---

## 📊 Dataset

El dataset incluye información de clientes de easyMoney con tres bloques de variables:

| Tipo | Variables |
|---|---|
| Sociodemográficas | Edad, género, nivel de estudios, ingresos |
| Comerciales | Segmento, canal de captación, antigüedad |
| De contratación | Productos contratados, historial de compra |

> ⚠️ Los datos son confidenciales y no se incluyen en este repositorio.

---

## 🔧 Metodología

El proyecto sigue un enfoque **end-to-end** en dos líneas paralelas de trabajo:

### 1. Data Cleaning & Preprocessing
- Tratamiento de valores nulos e inconsistencias.
- Normalización y codificación de variables.

### 2. Análisis Exploratorio (EDA)
- Distribución de variables sociodemográficas y comerciales.
- Análisis de correlación y patrones de contratación.
- Detección de segmentos naturales en los datos.

### 3. Feature Engineering
- Creación de variables derivadas a partir del historial de contratación.
- Selección de variables relevantes para cada modelo.
- Reducción de dimensionalidad con **PCA**.

### 4. Modelado
Dos notebooks independientes, uno por línea de trabajo:

| Notebook | Enfoque |
|---|---|
| `segmentacion_clientes.ipynb` | Clustering no supervisado (K-Means + PCA) |
| `propension_compra.ipynb` | Clasificación supervisada (propensión de compra) |

### 5. Evaluación
- Métricas de clustering: Silhouette Score, inercia.
- Métricas de clasificación: F1-Score, AUC-ROC, matriz de confusión.

### 6. Visualización y comunicación
- Dashboard interactivo en **Power BI** orientado al equipo comercial.
- Síntesis de insights accionables para negocio.

---

## 🧠 Modelos desarrollados

### 🔹 Segmentación de clientes
- Reducción de dimensionalidad con **PCA** para visualización y limpieza de ruido.
- Clustering con **K-Means** para identificar grupos de clientes con comportamientos diferenciados.
- Interpretación de cada segmento para su uso en estrategias comerciales.

### 🔹 Propensión de compra
- Feature engineering sobre variables sociodemográficas, comerciales y de contratación.
- Entrenamiento y comparación de modelos de clasificación.
- Optimización y evaluación orientada a negocio.

---

## 📈 Resultados e insights

- Identificación de segmentos de clientes con perfiles y necesidades diferenciadas, permitiendo personalizar la oferta comercial.
- Detección de patrones clave en la contratación de productos financieros.
- Modelo predictivo de propensión de compra con capacidad de priorizar leads de mayor valor.
- Generación de insights accionables presentados al área de negocio.

---

## 📊 Dashboard Power BI

Se desarrolló un dashboard interactivo orientado al área comercial con:

- KPIs principales de rentabilidad y volumen de clientes.
- Evolución temporal de ventas y contrataciones.
- Análisis en profundidad del perfil de cliente por segmento.
- Filtros dinámicos por canal, producto y perfil sociodemográfico.

🔗 **Acceder al dashboard** — disponible en la web del proyecto: [tfm0925grupo6.work](https://www.tfm0925grupo6.work/)

---

## 🛠️ Tech Stack

| Herramienta | Uso |
|---|---|
| `pandas` / `numpy` | Manipulación y operaciones numéricas |
| `scikit-learn` | Preprocesamiento, modelado y evaluación |
| `matplotlib` / `seaborn` | Visualización exploratoria |
| `Power BI` | Dashboard interactivo de negocio |
| `jupyter` | Entorno interactivo |
| `git` / `GitHub` | Control de versiones |

---

## 🚀 Instalación y uso

### Requisitos previos
- Python 3.8 o superior
- `pip`

### Pasos

```bash
# 1. Clona el repositorio
git clone https://github.com/christianirshool-glitch/My-projects.git
cd My-projects/TFM

# 2. Crea y activa un entorno virtual (recomendado)
python -m venv venv
source venv/bin/activate       # Linux/macOS
venv\Scripts\activate          # Windows

# 3. Instala las dependencias
pip install pandas numpy scikit-learn matplotlib seaborn jupyter

# 4. Lanza los notebooks
jupyter notebook notebooks/segmentacion_clientes.ipynb
jupyter notebook notebooks/propension_compra.ipynb
```

> ⚠️ Los datos no están disponibles públicamente. Para reproducir el análisis, sustituye los archivos en `data/` con un dataset propio de características similares.

---

## 📁 Estructura del proyecto

```
TFM/
├── data/                            # Datos (no incluidos)
├── notebooks/
│   ├── segmentacion_clientes.ipynb  # Clustering + PCA
│   └── propension_compra.ipynb      # Modelo predictivo
├── src/                             # Scripts auxiliares
├── models/                          # Modelos serializados
├── dashboard/                       # Archivo .pbix de Power BI
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
