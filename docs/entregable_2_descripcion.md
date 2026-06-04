# 🏎️ Proyecto Machine Learning: Predictor de Ventana de Pit Stops
## Etapa 2: Adquisición, Origen y Descripción del Dataset

### Materia: Aprendizaje Automático / Ciencia de Datos (2026)
### Profesor: Nicolas Caballero
### Estudiante: Martín Ozuna

---

> 📌 **Nota Metodológica:** Para construir un motor de consulta que emule con exactitud quirúrgica las ventanas de paradas en boxes, el algoritmo requiere entrenarse sobre un espacio muestral real y cerrado. Por este motivo, se toma la **Temporada 2024** como base estructural de ingeniería, dejando el pipeline preparado para la ingesta dinámica de la temporada 2025 en las próximas entregas.

---

### 🌐 1. Ecosistema de Fuentes Declaradas
Para mitigar el riesgo de escasez de variables y cumplir con el alcance predictivo del proyecto, se declara un entorno multifuente tanto estático como dinámico:

* **Kaggle F1 Dataset (Core Relacional):** Ingesta local de los archivos `races.csv`, `pit_stops.csv`, `lap_times.csv`, `drivers.csv`, `circuits.csv` y se incorpora `status.csv` para la auditoría de abandonos.
* **API Open-Meteo (Módulo Climático):** Fuente externa que proveerá el histórico de registros meteorológicos. Permitirá extraer e indexar la temperatura de pista, ambiente, humedad y probabilidad de precipitaciones mediante el cruce de las coordenadas geográficas de cada autódromo con las marcas temporales de las sesiones.
* **API Dinámica FastF1 & Historial de Comisarios de la FIA:** Registro oficial de telemetría y mensajes de dirección de carrera. Se utilizará para identificar de forma cronológica las neutralizaciones por banderas (verdes, amarillas, rojas) y la activación de *Safety Car* (SC) o *Virtual Safety Car* (VSC).
* **FastF1 API:** Reservado para la captura microscópica del identificador único del set físico de neumáticos y su edad en vueltas.
* **Pirelli Motorsport Database:** Matriz técnica de rangos térmicos operativos para compuestos C1 a C5.
* **Open-Meteo API:** Datos meteorológicos históricos complementarios por coordenadas geográficas de cada autódromo.

---

### 📊 2. Dimensión y Volumen del Dataset
Luego de ejecutar los cruces estructurales (*inner joins*) en el pipeline del Notebook, la matriz semilla consolidada presenta las siguientes propiedades métricas:

* **Cantidad total de instancias (filas):** `26.574` registros de vueltas en carrera.
* **Cantidad de características (columnas):** `11` variables técnicas iniciales.

---

### 📋 3. Diccionario de Datos Técnico (`df_f1_2025_raw`)

| # | Variable | Tipo de Dato | Estado | Descripción Técnica |
| :-: | :--- | :--- | :--- | :--- |
| **0** | `raceId` | `int64` | Non-Null | Identificador único de la ronda del campeonato. |
| **1** | `driverId` | `int64` | Non-Null | Código de registro interno de la FIA para el piloto. |
| **2** | `lap` | `int64` | Non-Null | Número de vuelta que transcurre en la carrera. |
| **3** | `position` | `int64` | Non-Null | Posición física del monoplaza en pista. |
| **4** | `time` | `object` (string) | Non-Null | Tiempo de vuelta en formato de telemetría. |
| **5** | `milliseconds` | `int64` | Non-Null | Tiempo de vuelta normalizado en milisegundos. |
| **6** | `circuitId` | `int64` | Non-Null | Código numérico de vinculación con el autódromo. |
| **7** | `grand_prix_name` | `object` (string) | Non-Null | Nombre comercial del Gran Premio disputado. |
| **8** | `round` | `int64` | Non-Null | Número de fecha correspondiente al calendario oficial. |
| **9** | `driver_name` | `object` (string) | Non-Null | Apellido identificador del piloto (ej. *Colapinto*). |
| **10** | `circuit_name` | `object` (string) | Non-Null | Nombre específico del trazado (ej. *Marina_Bay*). |

---

### ⚠️ 4. Control de Anomalías Estocásticas y Accidentes (Lógica de Curación)
Para garantizar que el modelo de Machine Learning aprenda el comportamiento y desgaste real de los neumáticos en condiciones competitivas y no se corrompa por eventos de fuerza mayor, se declaran tres protocolos de aislamiento de datos en la fase de curación:

1. **Clima y Ventanas Húmedas:** Las sesiones con precipitaciones serán segmentadas o penalizadas. Cuando la pista cambia de seco a mojado, la elección de compuestos (Intermedios o Full Wet) responde a una contingencia climática inmediata y no a la degradación física regular del neumático.
2. **Accidentes entre Pilotos (Colisiones y DNF):** Mediante el cruce con la tabla `status.csv`, se identificarán y removerán del espacio muestral las vueltas afectadas por choques, despistes o abandonos mecánicos. 
   * *Ejemplo metodológico:* Si un usuario consulta el modelo buscando la estrategia de un piloto (por ejemplo, *Bortoleto*) y el algoritmo calcula teóricamente que debería cambiar neumáticos en la vuelta 24, pero el resultado histórico de la carrera real dicta que el piloto hizo un **DNF** en esa misma vuelta porque se accidentó contra otro monoplaza, es un evento estocástico imposible de predecir por variables de desgaste. Por ende, estas instancias de colisión deben quedar completamente fuera del entrenamiento del modelo continuo.
3. **Accidentes Biológicos (Animales en Pista):** Eventos exógenos severos y aleatorios (como el ingreso de fauna local a la cinta asfáltica, ej. lagartijas, perros o aves, que obligan a desplegar banderas rojas o neutralizaciones imprevistas) serán identificados cruzando los reportes de incidentes de *FastF1*. Estas instancias serán aisladas de la matriz limpia, dado que corresponden a anomalías que rompen de manera abrupta cualquier patrón lógico de ritmo y simulación de carrera.

---

### 🎯 5. Definición de la Variable Target
El proyecto aborda un problema de **Aprendizaje Supervisado de Regresión**. Nuestra variable objetivo continua (Target) corresponde a la vuelta exacta del stint en la que el piloto ingresa al Pit Lane para cambiar sus compuestos, obtenida de la relación con `pit_stops.csv`. Al no presentar registros nulos, asegura un comportamiento estable para los modelos basados en árboles de decisión planteados.
