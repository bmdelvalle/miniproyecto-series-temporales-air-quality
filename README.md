# miniproyecto-series-temporales-air-quality
# Mini proyecto de Series Temporales - Calidad del Aire

## Descripción del proyecto

Este proyecto corresponde al mini proyecto de la unidad de **Series Temporales y Pronóstico** de Aprendizaje Automático II.

El objetivo es construir un flujo reproducible de forecasting para predecir la concentración diaria de benceno a partir de registros históricos de calidad del aire.

El dataset utilizado es **Air Quality - versión docente adaptada**, entregado para el curso. El trabajo se enfoca en la preparación temporal de los datos, análisis exploratorio, modelado univariante, evaluación temporal mediante backtesting, ajuste de hiperparámetros con Grid Search y comparación con un modelo que incorpora variables exógenas.

## Información del dataset

- Tema general: calidad del aire
- Archivo utilizado: `air_quality_docente.csv`
- Variable temporal: combinación de `fecha` + `hora`
- Variable objetivo: `concentracion_benceno`
- Frecuencia original: horaria
- Frecuencia de trabajo: diaria
- Horizonte de predicción: próximos 7 días

El dataset contiene registros horarios de contaminantes, sensores ambientales y variables meteorológicas. Para este proyecto, los datos fueron transformados a frecuencia diaria mediante agregación, respetando el orden cronológico de la serie temporal.

## Objetivo del modelo

Predecir la variable `concentracion_benceno` para los próximos 7 días utilizando modelos de forecasting implementados con `skforecast`.

El modelo principal es un modelo univariante, es decir, utiliza únicamente la historia pasada de la concentración de benceno para generar predicciones futuras.

También se incluye una comparación opcional con un modelo multivariante que utiliza variables exógenas, como sensores de contaminantes y variables ambientales.

## Metodología

El flujo de trabajo aplicado fue el siguiente:

1. Carga del dataset original.
2. Combinación de las columnas `fecha` y `hora` en una variable temporal.
3. Ordenamiento cronológico de los datos.
4. Definición del índice temporal.
5. Transformación de frecuencia horaria a frecuencia diaria.
6. Análisis exploratorio de la serie temporal.
7. Separación temporal entre entrenamiento y prueba.
8. Entrenamiento de un modelo base univariante.
9. Evaluación mediante backtesting.
10. Ajuste de lags e hiperparámetros mediante Grid Search.
11. Selección del mejor modelo univariante.
12. Entrenamiento de un modelo con variables exógenas.
13. Comparación de resultados.
14. Generación del pronóstico final a 7 días.
15. Guardado de datasets procesados, resultados y modelos entrenados.

## Modelos utilizados

### Modelo base univariante

Se entrenó un primer modelo de referencia utilizando la variable objetivo `concentracion_benceno` y sus valores pasados como predictores.

### Modelo univariante ajustado

Se aplicó Grid Search para evaluar distintas combinaciones de:

- Cantidad de lags.
- Hiperparámetros del regresor.
- Métricas de error.

La selección del mejor modelo se realizó principalmente usando **MAE** y también se reportó **RMSE**.

### Modelo con variables exógenas

Se entrenó un modelo adicional incorporando variables ambientales y señales de sensores como posibles predictores externos.

Variables consideradas como exógenas:

- `sensor_co`
- `sensor_nmhc`
- `sensor_nox`
- `sensor_no2`
- `sensor_o3`
- `temperatura`
- `humedad_relativa`
- `humedad_absoluta`

Este modelo se utilizó para comparar si las variables adicionales mejoraban el desempeño respecto al modelo univariante.

## Métricas de evaluación

Las métricas utilizadas fueron:

- **MAE**: error absoluto medio. Permite interpretar el error promedio en la misma escala que la concentración de benceno.
- **RMSE**: raíz del error cuadrático medio. Penaliza con mayor fuerza los errores grandes.

La evaluación se realizó respetando el orden temporal de los datos, evitando particiones aleatorias.

## Estructura del repositorio

```text
miniproyecto-series-temporales/
├── README.md
├── requirements.txt
├── informe.pdf
├── notebooks/
│   └── miniproyecto.ipynb
├── data/
│   ├── raw/
│   │   └── air_quality_docente.csv
│   └── processed/
│       └── air_quality_diario_procesado.csv
├── models/
│   ├── mejor_modelo_univariante.pkl
│   ├── mejor_modelo_exogenas.pkl
│   └── modelo_final_univariante.pkl
└── outputs/
    ├── comparacion_modelos.csv
    ├── evaluacion_final_test_7_dias.csv
    ├── grid_search_univariante.csv
    ├── grid_search_exogenas.csv
    └── pronostico_proximos_7_dias.csv
