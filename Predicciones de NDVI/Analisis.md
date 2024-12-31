# Análisis de Predicción del NDVI usando Prophet con Datos Satelitales

## Introducción

Este análisis examina la predicción del Índice de Vegetación de Diferencia Normalizada (NDVI) en la región de Molina de Aragón utilizando el modelo Prophet de Meta, junto con datos satelitales y climáticos. El NDVI es un indicador fundamental para monitorear la salud y el vigor de la vegetación, con valores que oscilan entre 0 y 1, donde valores más altos indican vegetación más saludable.

## Metodología

### Obtención de Datos

Los datos fueron obtenidos a través de Google Earth Engine (GEE) de dos fuentes principales:
1. Imágenes satelitales Sentinel-2 para el cálculo del NDVI
2. Datos climáticos de ERA5-Land para temperatura y precipitación

### Análisis Preliminar

El análisis inicial de la serie temporal del NDVI reveló un patrón interesante:

![Serie temporal NDVI](https://raw.githubusercontent.com/Deode22/Prophet-Predicting/main/Predicciones%20de%20NDVI/imagenes/serie-temp.png "Serie temporal NDVI 2017-2024")

Se identificaron tres caídas significativas en el NDVI:
- 2018-10-21: NDVI = 0.018 (Δ = -0.643)
- 2019-07-03: NDVI = 0.053 (Δ = -0.486)
- 2022-02-02: NDVI = 0.369 (Δ = -0.367)

Las caídas instantaneas (dos primeras) se identifican como errores o datos atípicos y se eliminan del análisis. A partir de 2022, las imagenes tienen tonalidades más nubladas, para los mismos parámetros. Esto se debe, con alta probabilidad, a la naturaleza del dataset, deprecado. 

Es por ello, que los datos mayores a 2022 se toman como erróneos y se utilizara Prophet para predecir los valores de esas fechas. Se compararán los resultados con valores NDVI calculador por el dataset harmonizado.

## Modelado con Prophet

### Optimización de Parámetros

Se realizó una búsqueda exhaustiva de parámetros para el modelo Prophet, considerando:
- changepoint_prior_scale: [0.001, 0.01, 0.1, 0.5]
- seasonality_prior_scale: [0.01, 0.1, 1.0, 10.0]
- seasonality_mode: ['multiplicative', 'additive']

Después de la validación cruzada, se seleccionaron los siguientes parámetros:
- changepoint_prior_scale: 0.20
- seasonality_prior_scale: 0.1
- seasonality_mode: 'additive'

### Resultados del Modelo

El modelo final mostró un rendimiento robusto:

Las métricas de rendimiento para el conjunto de datos histórico (hasta 2021):
- RMSE: 0.048
- MAE: 0.032

### Validación con Datos Harmonizados

Para validar el modelo, se compararon las predicciones con datos reales del producto COPERNICUS/S2_SR_HARMONIZED:

![Predicción del NDVI](https://raw.githubusercontent.com/Deode22/Prophet-Predicting/main/Predicciones%20de%20NDVI/imagenes/prediccion.png "Predicción NDVI usando Prophet")

Las métricas finales de validación fueron:
- RMSE: 0.054
- MAE: 0.038
- R²: 0.422

## Conclusiones

El modelo Prophet demostró ser efectivo para la predicción del NDVI, capturando tanto la tendencia general como los patrones estacionales. Las métricas de error son particularmente destacables considerando la alta sensibilidad del NDVI a las condiciones ambientales.

Los resultados sugieren que el modelo podría ser útil para:
1. Detección temprana de anomalías en la vegetación
2. Planificación de gestión forestal
3. Monitoreo de la salud vegetal a largo plazo

La diferencia entre las predicciones y los datos harmonizados valida la robustez del modelo, aunque también señala la importancia de considerar factores externos no incluidos en el modelo, como eventos climáticos extremos o cambios en el uso del suelo.
