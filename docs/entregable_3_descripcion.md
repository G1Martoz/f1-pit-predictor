# Entregable 3 – Análisis de Resultados del Modelo de Árbol de Decisión

## 1. Introducción

Como parte del desarrollo del proyecto de Aprendizaje Automático, se implementó un modelo de Árbol de Decisión con el objetivo de predecir la necesidad de realizar una parada en boxes (Pit Stop) durante una carrera de Fórmula 1. Para ello se utilizaron variables relacionadas con el desgaste de neumáticos, temperatura de pista y tipo de compuesto utilizado.

El modelo fue entrenado utilizando el algoritmo `DecisionTreeClassifier` de Scikit-Learn, configurado con una profundidad máxima de tres niveles (`max_depth=3`), criterio de entropía (`entropy`) y balanceo de clases mediante `class_weight='balanced'`.

---

## 2. Resultados Obtenidos

Las métricas obtenidas sobre el conjunto de prueba fueron las siguientes:

| Métrica   | Valor  |
| --------- | ------ |
| Accuracy  | 0.8462 |
| Precision | 0.2000 |
| Recall    | 1.0000 |
| F1-Score  | 0.3333 |

Asimismo, se registraron los siguientes valores de exactitud:

* Accuracy en entrenamiento: **0.7887**
* Accuracy en prueba: **0.8462**

La matriz de confusión obtenida fue:

|                   | Predicción: No Pit Stop | Predicción: Pit Stop |
| ----------------- | ----------------------- | -------------------- |
| Real: No Pit Stop | 21                      | 4                    |
| Real: Pit Stop    | 0                       | 1                    |

---

## 3. Análisis de las Métricas

### Accuracy

El modelo alcanzó una exactitud del **84.62%**, lo que indica que aproximadamente ocho de cada diez observaciones fueron clasificadas correctamente.

A simple vista este valor podría considerarse satisfactorio; sin embargo, la exactitud puede resultar engañosa en problemas donde existe un fuerte desbalance de clases, como ocurre en este caso. La mayoría de las observaciones corresponden a vueltas normales de carrera y solamente una pequeña proporción representa eventos de entrada a boxes.

Por este motivo fue necesario complementar el análisis con otras métricas.

---

### Precision

La precisión obtenida fue de **20%**.

Esto significa que de todas las ocasiones en las que el modelo predijo una entrada a boxes, solamente el 20% fueron efectivamente paradas reales.

En términos prácticos, el modelo genera una cantidad considerable de falsas alarmas. Es decir, identifica situaciones como potenciales Pit Stops cuando en realidad el piloto continuó en pista.

Este comportamiento es consistente con el uso de la estrategia de balanceo de clases (`class_weight='balanced'`), cuyo objetivo principal es evitar que los eventos minoritarios sean ignorados.

---

### Recall

El Recall alcanzó un valor de **100%**.

Esta métrica indica que el modelo fue capaz de identificar correctamente todos los eventos reales de entrada a boxes presentes en el conjunto de prueba.

Desde una perspectiva estratégica, este resultado es particularmente relevante, ya que significa que el sistema no dejó escapar ninguna parada real.

En otras palabras, cada vez que ocurrió un Pit Stop en los datos de prueba, el modelo logró detectarlo.

---

### F1-Score

El valor obtenido para F1-Score fue de **0.3333**.

Esta métrica representa el equilibrio entre Precision y Recall.

Aunque el Recall fue perfecto, la baja Precision redujo considerablemente el valor final del F1-Score. Esto evidencia que el modelo prioriza detectar todas las paradas posibles, incluso a costa de generar múltiples falsos positivos.

Por lo tanto, el F1-Score revela que todavía existe margen de mejora para alcanzar un equilibrio más adecuado entre sensibilidad y precisión.

---

## 4. Interpretación de la Matriz de Confusión

La matriz de confusión permite comprender con mayor detalle el comportamiento del modelo:

* Verdaderos Negativos (TN): 21
* Falsos Positivos (FP): 4
* Falsos Negativos (FN): 0
* Verdaderos Positivos (TP): 1

### Hallazgos

#### Verdaderos Positivos

El único evento real de Pit Stop presente en el conjunto de prueba fue correctamente identificado por el modelo.

Esto explica el Recall del 100%.

#### Falsos Negativos

No se registraron falsos negativos.

Este es un resultado muy positivo para el objetivo del proyecto, ya que implica que ninguna parada real pasó desapercibida.

#### Falsos Positivos

Se observaron cuatro falsos positivos.

Estas situaciones corresponden a vueltas en las que el modelo recomendó una entrada a boxes cuando realmente el piloto permaneció en pista.

Este comportamiento explica la baja Precision obtenida.

#### Verdaderos Negativos

La mayoría de las vueltas normales fueron correctamente clasificadas, con 21 aciertos sobre 25 casos negativos.

Esto demuestra que el modelo también posee una capacidad razonable para reconocer situaciones donde no es necesario realizar una parada.

---

## 5. Hallazgos Estratégicos

El árbol de decisión permitió identificar que variables relacionadas con:

* Antigüedad de los neumáticos (`tyre_age_laps`)
* Degradación estimada (`degradacion_calculada`)
* Temperatura de pista (`track_temp`)
* Compuesto utilizado (`compound_encoded`)

aportan información relevante para la toma de decisiones estratégicas de Pit Stop.

Los resultados sugieren que el modelo aprendió patrones asociados al desgaste progresivo de neumáticos y a las condiciones de carrera que suelen preceder una parada en boxes.

Además, el comportamiento observado indica que el árbol fue configurado para ser conservador respecto a la detección de Pit Stops, priorizando no perder oportunidades de parada antes que minimizar falsas alarmas.

---

## 6. Limitaciones

Durante el análisis se identificaron algunas limitaciones:

* Existe un fuerte desbalance entre vueltas normales y eventos de Pit Stop.
* El conjunto de prueba contiene únicamente un evento positivo real, lo que dificulta una evaluación estadísticamente robusta.
* El modelo utiliza un número limitado de variables y no incorpora información contextual adicional como tráfico, Safety Car, degradación real de telemetría o estrategia del equipo.
* El árbol de decisión, aunque interpretable, puede tener menor capacidad predictiva que modelos más complejos.

---

## 7. Conclusiones

El modelo de Árbol de Decisión logró identificar correctamente todos los eventos de Pit Stop presentes en el conjunto de prueba, alcanzando un Recall del 100%.

La exactitud general fue del 84.62%, mostrando un desempeño razonablemente bueno para una primera aproximación al problema. Sin embargo, la baja Precision y el F1-Score obtenido indican que todavía existe una tendencia a generar falsas alarmas.

Desde una perspectiva operativa, el modelo resulta útil como herramienta de apoyo para detectar posibles ventanas de parada, ya que minimiza el riesgo de omitir eventos importantes. No obstante, futuras iteraciones deberían enfocarse en reducir la cantidad de falsos positivos mediante técnicas adicionales de ingeniería de características, ajuste de hiperparámetros y experimentación con modelos más avanzados.

En conclusión, los resultados obtenidos demuestran que las variables seleccionadas contienen información relevante para la predicción de Pit Stops y constituyen una base sólida para continuar mejorando el sistema de apoyo a decisiones estratégicas en Fórmula 1.
