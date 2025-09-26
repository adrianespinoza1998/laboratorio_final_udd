# Laboratorio 3 — Series Temporales

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
