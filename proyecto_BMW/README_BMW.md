# 🚗 Análisis de Precios de Coches BMW de Segunda Mano

![Python](https://img.shields.io/badge/Python-3.8%2B-blue?logo=python&logoColor=white)
![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-orange?logo=jupyter&logoColor=white)
![Scikit-learn](https://img.shields.io/badge/Scikit--learn-ML-red?logo=scikit-learn&logoColor=white)
![License](https://img.shields.io/badge/Licencia-MIT-green)

Proyecto completo de ciencia de datos que explora los factores que determinan el precio de coches BMW de segunda mano. Incluye limpieza de datos, imputación inteligente por similitud, análisis exploratorio, ingeniería de características y modelado predictivo con Random Forest.

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

El mercado de segunda mano de coches premium presenta una alta variabilidad en precios influida por múltiples factores: modelo, antigüedad, kilometraje, equipamiento y tipo de combustible. Entender qué variables tienen mayor impacto permite tomar mejores decisiones tanto desde la perspectiva del comprador como del vendedor.

---

## 🎯 Objetivo

Construir un modelo predictivo capaz de estimar el precio de un BMW de segunda mano a partir de sus características, e identificar los factores con mayor influencia en el precio.

---

## 📊 Dataset

El dataset `bmw_pricing_v3.csv` contiene aproximadamente **5.000 registros** con las siguientes columnas:

| Columna | Descripción |
|---|---|
| `precio` | Variable objetivo — precio en euros |
| `modelo` | Modelo BMW (e.g., X3, Serie 3) |
| `km` | Kilómetros recorridos |
| `potencia` | Caballos de potencia (HP) |
| `tipo_coche` | Tipo de carrocería (SUV, berlina, etc.) |
| `tipo_gasolina` | Tipo de combustible (diésel, gasolina, eléctrico) |
| `color` | Color exterior |
| `fecha_registro` | Fecha de primera matriculación |
| `fecha_venta` | Fecha de venta |
| `aire_acondicionado`, `gps`, … | Equipamiento (variables booleanas) |

> ⚠️ El dataset no se incluye en este repositorio. Colócalo en `Datasets/` o modifica la ruta en el notebook.

---

## 🔧 Metodología

### 1. Carga e inspección inicial
Carga del CSV, revisión de dimensiones, tipos de datos y valores nulos. Las columnas de fecha se convierten de `float` a `datetime`.

### 2. Imputación de valores nulos
Se utiliza una función personalizada `utils.imputar_por_similitud` basada en similitud de `km` y `potencia`:
- **Ajuste estricto**: tolerancias pequeñas para equipamiento y variables estructurales.
- **Ajuste relajado**: para categorías restantes.
- **Fallback con mediana**: para fechas, con `km` como criterio secundario.

Tras la imputación se eliminan filas con más de un nulo y las fechas se normalizan al primer día del mes.

### 3. Ingeniería de características
- Cálculo de **antigüedad** (años) a partir de las fechas de registro y venta.
- Corrección de nombres de modelo inconsistentes (e.g., `Active Tourer` → `218 Active Tourer`).
- Homogeneización de valores de combustible y tipo de carrocería.

### 4. Tratamiento de outliers
- Eliminación de precios > 100.000 € y < 0 €.
- Eliminación de kilómetros > 500.000 km; valores negativos imputados con la mediana del modelo.
- Eliminación de potencias < 50 HP.
- Eliminación de filas donde la fecha de registro es posterior a la de venta.

### 5. Análisis exploratorio (EDA)
- **Univariante**: histogramas para variables numéricas, gráficos de barras para categóricas.
- **Bivariante**: gráficos de regresión (km, potencia, antigüedad vs. precio), boxplots por equipamiento, combustible, modelo y color.
- **Correlación**: Pearson/Spearman para variables numéricas, V de Cramér para asociaciones entre categóricas.

### 6. Preprocesamiento para modelado
- **One-Hot Encoding** para `color`, `tipo_gasolina`, `tipo_coche` y `modelo` (~50 columnas binarias).
- **Min-Max Scaling** para `km`, `potencia` y `antigüedad`.

### 7. Modelado predictivo
- Entrenamiento de un **Random Forest Regressor** con 50 árboles.
- Evaluación mediante validación cruzada de 3 pliegues (métrica R²).

---

## 📈 Resultados clave

| Métrica | Valor |
|---|---|
| R² (validación cruzada) | **≈ 0.91** |

- **Modelo** es el predictor con mayor influencia en el precio.
- **Antigüedad** y **km** presentan correlación negativa con el precio.
- **Potencia** muestra correlación positiva.
- El equipamiento GPS y cámara trasera tiende a incrementar el precio.
- Los modelos eléctricos e híbridos (e.g., i3) presentan patrones de precio diferenciados.
- El tipo de combustible y la carrocería tienen influencia moderada.

---

## 🛠️ Tech Stack

| Librería | Uso |
|---|---|
| `pandas` | Manipulación de datos |
| `numpy` | Operaciones numéricas |
| `matplotlib` / `seaborn` | Visualización |
| `scikit-learn` | Preprocesamiento, modelado y evaluación |
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
cd My-projects/proyecto_BMW

# 2. Crea y activa un entorno virtual (recomendado)
python -m venv venv
source venv/bin/activate       # Linux/macOS
venv\Scripts\activate          # Windows

# 3. Instala las dependencias
pip install pandas numpy scikit-learn matplotlib seaborn jinja2 jupyter

# 4. Coloca el dataset
mkdir Datasets
# Copia bmw_pricing_v3.csv dentro de Datasets/

# 5. Lanza el notebook
jupyter notebook bmw.ipynb
```

> 📌 El notebook importa `utils.py`, que contiene la función `imputar_por_similitud`. Asegúrate de tener este archivo en el mismo directorio o adapta la lógica de imputación según tus necesidades.

---

## 📁 Estructura del proyecto

```
proyecto_BMW/
├── Datasets/
│   └── bmw_pricing_v3.csv       # Dataset (no incluido)
├── bmw.ipynb                    # Notebook principal
├── utils.py                     # Funciones auxiliares de imputación
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
