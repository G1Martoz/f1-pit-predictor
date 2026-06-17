# VIDEO DE YOUTUBE:
[VIDEO DE YOUTUBE](https://youtu.be/KZopIQugPAs)

# 🏎️ Predictor de Ventana Óptima de Pit Stops (F1)
Este proyecto de Machine Learning aplica algoritmos de aprendizaje supervisado (regresión) para determinar con precisión numérica la vuelta exacta en la que un monoplaza debe realizar su detención en boxes. El modelo funciona como un motor de consulta de realidad basado en el historial técnico del campeonato 2024.

---

## 🧭 Guía de Navegación del Repositorio

Para facilitar la auditoría académica del proyecto, el espacio de trabajo se ha estructurado bajo el estándar industrial de **Cookiecutter Data Science**. Podés explorar los componentes clave navegando a través de los siguientes accesos directos:

* **📂 [data/raw/](./Data/raw/)**: Bandeja de entrada de datos crudos. Contiene las 5 fuentes relacionales estáticas extraídas originalmente desde Kaggle (`races.csv`, `pit_stops.csv`, `lap_times.csv`, `drivers.csv`, `circuits.csv`).
* **📓 [Notebooks/1.0-mo-descripcion-origen-dataset.ipynb](./Notebooks/1.0-mo-descripcion-origen-dataset.ipynb)**: Cuaderno de desarrollo principal. Contiene el esqueleto jerárquico del pipeline de datos, el aislamiento de la muestra y la ejecución del método `.info()` que computa las **26.574 instancias** activas.
* **📄 [docs/entregable_2_descripcion.md](./docs/entregable_2_descripcion.md)**: **Reporte Técnico de la Entrega 2**. Documento estilizado que detalla el ecosistema multifuente, el volumen dimensional del espacio muestral y el Diccionario de Datos explícito con la definición del Target continuo.
* **⚙️ [requirements.txt](./requirements.txt)**: Archivo de configuración del entorno virtual con las declaraciones de dependencias y librerías del proyecto.

---

## 🚀 Instrucciones de Inicialización Local

Si deseas clonar este repositorio y reproducir el entorno de análisis, ejecutá la siguiente secuencia de comandos en tu terminal:

# 1. Clonar el repositorio remoto
git clone <https://github.com/G1Martoz/f1-pit-predictor.git>

# 2. Instalar el ecosistema de librerías requeridas
pip install -r requirements.txt