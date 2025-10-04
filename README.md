# Laboratorio Final

## Contexto
Egresos hospitalarios por **FECHAALTA**, frecuencia **diaria**, año **2024** (cobertura 100%).

## Flujo (RA1–RA3)
#1. **RA1 — Auditoría temporal**: columna `FECHAALTA`, rango 2024-01-01 a 2024-12-31 (bisiesto), sin huecos ni outliers.
#2. **RA2 — EDA y descomposición**: patrón semanal dominante (viernes alto, domingo bajo); mensualidad secundaria; STL con amplitud ~90% del nivel medio.
#3. **RA3 — Modelado y validación temporal**:
 #  - **Partición**: Train = 2024-01-01→2024-12-03; Test = 2024-12-04→2024-12-31 (28 días).
  # - **Baselines**: Naïve y Seasonal Naïve (m=7).
   #- **Modelos**: SARIMA(1,1,1)(0,1,1)[7] y ETS aditivo (tendencia + estacionalidad semanal).
   #- **Métricas globales (RMSE)**: Naïve = 61.31; S-Naïve = 30.30; SARIMA = 31.13; ETS = 31.42.
   #- **Por horizonte (h=1…7)**: errores más altos en h=1 (lunes) y mejora en h=4–6.

## Reproducibilidad (RA5)
#- **Estructura**: ver carpetas en este repo (`data/`, `src/`, `reports/`, `notebooks/`).
#- **Cómo correr**:
#  1) Instalar dependencias: `pip install -r requirements.txt`
 # 2) Abrir y ejecutar `notebooks/Lab3_work.ipynb`
 # 3) Gráficos se exportan a `reports/figures/` y tablas a `reports/tables/`
#- **Decisiones claves**:
 # - Serie diaria por `FECHAALTA`; no se aplicó log/diferencias adicionales.
 # - Validación con **holdout temporal** (28 días).
  #- **Intervalos**: SARIMA nativos; ETS con desviación estándar de residuos (σ≈20.66).
#- **Limitaciones**:
 # - Días atípicos (feriados 24–25 dic) no modelados explícitamente; considerar exógenas en SARIMAX.

## Archivos generados
#- `data/processed/egresos_diarios_2024.csv`
#- `reports/tables/metrics_global.csv` (+ métricas por horizonte)
#- `reports/figures/` (EDA, STL, pronósticos)

# Laboratorio Final: Análisis Exploratorio de Egresos Hospitalarios

## Contexto

Este repositorio corresponde al **Laboratorio Final**, que consiste en un análisis exploratorio de un dataset de egresos hospitalarios, con un enfoque en las diferencias por sexo. El análisis se centra en datos del año 2024 y utiliza la fecha de alta (`FECHAALTA`) como eje temporal principal.

El análisis completo y el código se encuentran en el notebook `notebooks/data_quality_report_EDA2.ipynb`.

## Flujo del Análisis

El flujo de trabajo se divide en dos fases principales:

### 1. Fase 1: Auditoría Temporal

-   **Selección de Columna Temporal:** Se identifica y selecciona `FECHAALTA` como la columna de tiempo más adecuada para el análisis de series temporales.
-   **Análisis de Calidad:** Se audita la serie temporal para el año 2024, confirmando una alta densidad de datos (99.5%), sin valores nulos y con solo un outlier detectado, lo que indica una serie de tiempo de alta calidad.
-   **Frecuencia:** Se establece una frecuencia diaria para el análisis.

### 2. Fase 2: Análisis Exploratorio de Datos (EDA) por Sexo

-   **Limpieza y Preprocesamiento:**
    -   Se manejan valores nulos, imputando con la media para variables numéricas y la moda para categóricas.
    -   Se eliminan columnas con un alto porcentaje de valores faltantes.
    -   Se corrigen y estandarizan los valores de la columna `SEXO`.
-   **Visualización y Análisis Comparativo:**
    -   Se generan visualizaciones para comparar la distribución de variables clave entre hombres y mujeres.
    -   **Gráficos generados:** Boxplots, gráficos de barras, violin plots y gráficos de torta para visualizar las distribuciones de severidad, procedimientos y datos demográficos.

## Hallazgos Clave

-   **Distribución por Sexo:** La muestra se compone de un 58.1% de mujeres y un 41.9% de hombres.
-   **Severidad de Casos:** La mayoría de los pacientes (tanto hombres como mujeres) se encuentran en niveles de severidad 2 y 3 (moderada a grave). Las mujeres muestran una mayor concentración en severidad 2, mientras que los hombres la tienen en severidad 3.
-   **Procedimientos Médicos:**
    -   Los promedios de los procedimientos (`PROCEDIMIENTO1`, `PROCEDIMIENTO5`) son muy similares entre ambos sexos.
    -   Se observa una mayor variabilidad (desviación estándar más alta) en los procedimientos aplicados a los hombres, lo que podría sugerir una mayor diversidad en la complejidad de los casos.

## Reproducibilidad

1.  **Instalar dependencias:**
    ```bash
    pip install pandas matplotlib seaborn statsmodels
    ```
2.  **Ejecutar el análisis:**
    Abrir y ejecutar el notebook `notebooks/data_quality_report_EDA2.ipynb`. Las figuras generadas se guardan en la carpeta `reports/figures/`.

## Limitaciones

-   Los códigos de procedimientos (ej. `PROCEDIMIENTO1`) están en formato CIE-9-MC y no incluyen descripciones textuales en el dataset. Para interpretarlos, se requeriría un diccionario de datos externo.
-   El análisis se limita a las variables presentes en el dataset y no considera factores externos que podrían influir en los egresos hospitalarios.
