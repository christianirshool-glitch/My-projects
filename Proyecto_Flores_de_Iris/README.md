📊 # *Clasificación de Flores Iris*

📌 *Descripción*

Este proyecto desarrolla modelos de Machine Learning para clasificar flores iris en tres especies (Setosa, Versicolor y Virginica), combinando enfoques supervisados y no supervisados. Se sigue un flujo completo de ciencia de datos: análisis exploratorio, modelado, evaluación y comparación de técnicas.

🎯 *Objetivo*

Construir un modelo capaz de predecir con alta precisión la especie de una flor iris y analizar el comportamiento de diferentes enfoques de aprendizaje automático (clasificación y clustering).

📂 *Dataset*

· Dataset clásico Iris

· Variables:
  - Longitud y anchura del sépalo
  - Longitud y anchura del pétalo
· Variable objetivo:
  - Especie (Setosa, Versicolor, Virginica)
🔍 *Flujo del proyecto*
1. Análisis Exploratorio (EDA)
  - Identificación de patrones y relaciones entre variables
  - Visualización de la separabilidad entre especies
  - Detección de solapamiento entre clases
2. Modelado Supervisado (SVM)
  - Entrenamiento de un clasificador SVM con kernel lineal
  - Evaluación con métricas de clasificación
3. Modelado No Supervisado (K-Means)
  - Aplicación de clustering con y sin escalado
  - Evaluación mediante ARI y Silhouette Score
4. Evaluación y Validación
  - Matriz de confusión
  - Classification report
  - Validación cruzada

🧠 *Resultados*

🔹 Modelo Supervisado (SVM)
·  Precisión en test: 100%
·  Validación cruzada: 98% (±1.63%)

Matriz de confusión:

[[19  0  0]

 [ 0 13  0]
 
 [ 0  0 13]]

Classification report:

precision    recall  f1-score   support

setosa       1.00      1.00      1.00        19
versicolor   1.00      1.00      1.00        13
virginica    1.00      1.00      1.00        13

👉 El modelo muestra una capacidad perfecta de clasificación en test, confirmando la separabilidad de los datos.


🔹 *Modelo No Supervisado (K-Means)*

· Sin escalado:

  - ARI: 0.72
  - Silhouette Score: 0.55

· Con escalado:

  - ARI: 0.43
  - Silhouette Score: 0.48

👉 El escalado empeoró los resultados, indicando que las variables originales ya estaban en una escala adecuada.


📊 *Insights clave*

·  Iris setosa es fácilmente separable del resto debido a sus características de pétalo.
·  Existe cierto solapamiento entre versicolor y virginica, lo que supone mayor complejidad.
·  Los modelos supervisados (SVM) superan claramente a los no supervisados en este problema.
·  La selección del preprocesamiento (como el escalado) puede afectar negativamente si no es necesario.


🛠️ *Tecnologías utilizadas*

·  Python
·  Pandas
·  Numpy
·  Scikit-learn
·  Matplotlib / seaborn

👤 *Autor:* 

Christian Méndez Giraldo
