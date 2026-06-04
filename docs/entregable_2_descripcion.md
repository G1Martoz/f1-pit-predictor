# 🏎️ Proyecto Machine Learning: Predictor de Ventana de Pit Stops
## Etapa 2: Adquisición, Origen y Descripción del Dataset

### Materia: Aprendizaje Automático / Ciencia de Datos (2026)
### Profesor: Nicolas Caballero
### Estudiante: Martín Ozuna

---

> 📌 **Nota Metodológica:** Para construir un motor de consulta que emule con exactitud quirúrgica las ventanas de paradas en boxes, el algoritmo requiere entrenarse sobre un espacio muestral real y cerrado. Por este motivo, se toma la **Temporada 2024** como base estructural de ingeniería, dejando el pipeline preparado para la ingesta dinámica de la temporada 2025 en las próximas entregas.

---

### 🌐 1. Ecosistema de Fuentes Declaradas
Para mitigar el riesgo de escasez de variables en fases avanzadas del modelo, se ha diseñado un entorno multifuente de dominio público y técnico:

* **Kaggle F1 Dataset (Core Relacional):** Ingesta local de los archivos `races.csv`, `pit_stops.csv`, `lap_times.csv`, `drivers.csv` y `circuits.csv`.
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

### 🎯 4. Definición de la Variable Target
El proyecto aborda un problema de **Aprendizaje Supervisado de Regresión**. Nuestra variable objetivo continua (Target) corresponde a la vuelta exacta del stint en la que el piloto ingresa al Pit Lane para cambiar sus compuestos, obtenida de la relación con `pit_stops.csv`. Al no presentar registros nulos, asegura un comportamiento estable para los modelos basados en árboles de decisión planteados.
