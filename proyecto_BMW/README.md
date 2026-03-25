# *Análisis de Precios de Coches BMW de Segunda Mano*

https://img.shields.io/badge/Python-3.8%252B-blue

https://img.shields.io/badge/Jupyter-Notebook-orange

https://img.shields.io/badge/Licencia-MIT-green

Proyecto completo de ciencia de datos que explora los factores que influyen en el precio de coches BMW usados. El notebook incluye limpieza de datos, imputación inteligente, análisis exploratorio, ingeniería de características y modelado predictivo.

📌 *Índice*

· Descripción general
· Dataset
· Flujo de trabajo
· Principales hallazgos
· Tecnologías utilizadas
· Instalación y uso
· Estructura del repositorio
· Trabajo futuro
· Autor

📊 *Descripción general*

Este proyecto tiene como objetivo entender qué determina el precio de los BMW de segunda mano. Utilizando un conjunto de datos con especificaciones técnicas, equipamiento e información de venta, se realizan las siguientes tareas:

· Limpieza e imputación de valores nulos mediante un enfoque basado en similitud (KNN).
· Creación de nuevas características (por ejemplo, antigüedad).
· Análisis exploratorio de datos para descubrir patrones.
· Codificación de variables categóricas con One‑Hot Encoding.
· Escalado de variables numéricas con Min‑Max Scaler.
· Entrenamiento de un Random Forest Regressor para predecir el precio.
· Evaluación del modelo mediante validación cruzada.

📁 *Dataset*

El dataset original bmw_pricing_v3.csv contiene alrededor de 5000 registros con las siguientes columnas:

Columna |	Descripción
precio |	Variable objetivo – precio en euros
modelo |	Modelo BMW (ej. X3, Serie 3)
km |	Kilómetros recorridos
potencia |	Caballos de potencia (HP)
tipo_coche |	Tipo de carrocería (SUV, berlina, etc.)
tipo_gasolina |	Tipo de combustible (diésel, gasolina, eléctrico)
color	| Color exterior
fecha_registro |	Fecha de primera matriculación
fecha_venta |	Fecha de venta
aire_acondicionado, gps, …| 	Características de equipamiento (booleanas)
Nota: El dataset no se incluye en este repositorio. Debes colocarlo en una carpeta Datasets/ o modificar la ruta en el notebook.



🔧 # *Flujo de trabajo*
1. Carga e inspección inicial
· Se carga el CSV y se comprueban dimensiones, valores nulos y tipos de datos.
· Las columnas de fecha se convierten de float a datetime.



2. *Imputación de valores nulos*
· Se utiliza una función personalizada utils.imputar_por_similitud que busca filas similares (basada en km y potencia) para rellenar los nulos.
· Tres etapas:

  - Ajuste estricto (pequeñas tolerancias en km y potencia) para equipamiento y variables estructurales.

  - Ajuste relajado para las categorías restantes.

  - Imputación con mediana para fechas, utilizando coches similares o solo km como fallback.

· Después de la imputación, se eliminan filas con más de un nulo y las fechas se normalizan al primer día del mes.



3. *Ingeniería de características*
· Se calcula antigüedad (años) a partir de las fechas de registro y venta.
· Corrección de nombres de modelo inconsistentes (ej. Active Tourer → 218 Active Tourer).
· Homogeneización de combustibles (Diesel → diesel).
· Corrección de tipo de carrocería (para el X3 que aparecía como van se cambia a suv).




4. *Tratamiento de valores atípicos*
· Eliminación de precios > 100.000 € y < 0 €.
· Eliminación de kilómetros > 500.000 km; los km negativos se imputan con la mediana del modelo.
· Eliminación de potencias < 50 HP (imputadas posteriormente).
· Eliminación de filas donde la fecha de registro es posterior a la de venta.




5. *Análisis exploratorio (EDA)*
· Análisis univariante: histogramas para variables numéricas, gráficos de barras para categóricas.
· Análisis bivariante:

  - Gráficos de regresión (km, potencia, antigüedad vs precio) – tendencias lineales y logarítmicas.
  - Boxplots para comparar la distribución de precios según equipamiento, combustible, tipo de coche, color y modelo.

· Análisis de correlación:

  - Pearson/Spearman para variables numéricas.
  - V de Cramér para asociaciones entre categóricas.
  -  Matrices de correlación con la variable objetivo para identificar los predictores más influyentes.



6. *Codificación y escalado*
· One‑Hot Encoding para color, tipo_gasolina, tipo_coche y modelo (se generan ~50 columnas binarias).
· Min‑Max Scaling para km, potencia y antigüedad.



7. *Modelado*
· Entrenamiento de un Random Forest Regressor con 50 árboles.
· Evaluación mediante validación cruzada de 3 pliegues (métrica R²).
· R² alcanzado ≈ 0.91 – excelente capacidad predictiva.



📈 *Principales hallazgos*
· Modelo es el predictor más fuerte del precio (como era de esperar).
· Antigüedad y km muestran una correlación negativa con el precio.
· Potencia tiene correlación positiva con el precio.
· Características de equipamiento como gps y cámara trasera tienden a aumentar el precio.
· Los modelos eléctricos e híbridos (como el i3) presentan patrones de precio distintos.
· El tipo de combustible y la carrocería tienen una influencia moderada.



🛠 # *Tecnologías utilizadas*
· Python 3.8+
· Pandas – manipulación de datos
· NumPy – operaciones numéricas
· Matplotlib / Seaborn – visualización
· Scikit‑learn – preprocesamiento, modelado, evaluación
· Jupyter Notebook – entorno interactivo

🚀 *Instalación y uso*
*Requisitos previos*
· Python 3.8 o superior
· pip (gestor de paquetes)

*Configuración*
1. Clona este repositorio:

bash
git clone https://github.com/tuusuario/bmw-price-analysis.git
cd bmw-price-analysis


2. Crea un entorno virtual (opcional pero recomendado):

bash
python -m venv venv
source venv/bin/activate   # Linux/macOS
venv\Scripts\activate      # Windows


3. Instala las dependencias:

bash
pip install pandas numpy scikit-learn matplotlib seaborn jinja2


4. Coloca el archivo de datos bmw_pricing_v3.csv dentro de una carpeta Datasets/ (o modifica la ruta en el notebook).


*Ejecución*
Lanza Jupyter Notebook:

bash
jupyter notebook bmw.ipynb


Ejecuta las celdas en orden. El notebook:

· Cargará los datos.
· Imputará valores nulos (requiere utils.py – ver nota).
· Realizará el análisis.
· Mostrará los resultados.

*Utilidades personalizadas*
El notebook importa un módulo utils que contiene la función imputar_por_similitud. Este archivo no se incluye en este repositorio, pero puedes encontrarlo en mi repositorio de utilidades o escribir tu propia lógica de imputación.



👤 *Autor:* 
Christian Méndez Giraldo


📄 *Licencia*
Este proyecto está bajo la licencia MIT – consulta el archivo LICENSE para más detalles.
