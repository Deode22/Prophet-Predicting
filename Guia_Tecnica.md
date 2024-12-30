# Guía Completa de Parámetros de Prophet

## Introducción

La efectividad de Prophet en el pronóstico de series temporales depende fundamentalmente de una correcta configuración de sus parámetros. Esta guía proporciona una visión detallada de los parámetros clave y su impacto en el rendimiento del modelo, con consideraciones específicas para el análisis de series temporales.

## Parámetros Fundamentales

### Parámetro de Crecimiento
El parámetro `growth` (predeterminado: 'linear') determina el patrón básico de la tendencia:

El crecimiento lineal resulta adecuado para la mayoría de los escenarios con progresión constante. El crecimiento logístico es ideal para datos acotados, como valores NDVI entre 0 y 1. El crecimiento plano se adapta mejor a series estacionarias.

### Configuración de Puntos de Cambio
El `changepoint_prior_scale` (predeterminado: 0.05) gobierna la flexibilidad de la tendencia:

Para entornos de alta estabilidad, como bosques establecidos, se recomiendan valores más bajos (0.001-0.01), lo que resulta en tendencias más suaves y minimiza el riesgo de sobreajuste. En entornos dinámicos, valores más altos (0.1-0.5) capturan eficazmente los cambios abruptos, siendo esencial para detectar eventos de perturbación.

### Control de Estacionalidad

El `seasonality_prior_scale` (predeterminado: 10.0) gestiona la intensidad de los patrones estacionales:

Para regiones con estacionalidad marcada:
```python
model = Prophet(seasonality_prior_scale=15.0)  # Valores entre 15-30
```

Para regiones con estacionalidad débil:
```python
model = Prophet(seasonality_prior_scale=5.0)   # Valores entre 1-10
```

El parámetro `seasonality_mode` define el tipo de interacción estacional:
```python
model = Prophet(
    seasonality_mode='multiplicative'  # Para sistemas naturales
    # o
    seasonality_mode='additive'        # Para cambios estacionales constantes
)
```

## Parámetros Avanzados de Estacionalidad

### Control de Patrones Anuales
El parámetro `yearly_seasonality` ofrece una modelización estacional flexible:

```python
model = Prophet(
    yearly_seasonality=20  # Recomendado para ciclos de vegetación típicos
)
```

### Granularidad Temporal
Para análisis de datos mensuales:
```python
model = Prophet(
    weekly_seasonality=False,
    daily_seasonality=False
)
```

## Configuración Optimizada para NDVI

```python
model = Prophet(
    growth='linear',
    changepoint_prior_scale=0.05,
    seasonality_prior_scale=15.0,
    seasonality_mode='multiplicative',
    yearly_seasonality=20,
    weekly_seasonality=False,
    daily_seasonality=False,
    interval_width=0.95,
    mcmc_samples=300
)
```

## Optimización de Parámetros

Para una selección sistemática de parámetros:

```python
param_grid = {
    'changepoint_prior_scale': [0.001, 0.01, 0.05, 0.1],
    'seasonality_prior_scale': [1.0, 5.0, 10.0, 15.0],
    'seasonality_mode': ['multiplicative'],
    'yearly_seasonality': [10, 15, 20]
}
```

La selección final de parámetros debe considerar cuatro aspectos fundamentales: las características específicas de la serie temporal, los objetivos del análisis, los recursos computacionales disponibles y los requisitos de precisión necesarios.

### Consideraciones para la Validación

La validación cruzada resulta esencial para garantizar la robustez del modelo. Se recomienda utilizar períodos de validación que representen adecuadamente los ciclos naturales del fenómeno estudiado. Para datos de vegetación, esto implica típicamente períodos de validación de al menos un año completo.

### Interpretación de Resultados

La interpretación de los resultados debe considerar el contexto ecológico. Los cambios detectados en la tendencia pueden indicar transformaciones importantes en el ecosistema, mientras que las variaciones en los patrones estacionales pueden reflejar alteraciones en los ciclos fenológicos naturales.

Este enfoque sistemático para la selección de parámetros asegura resultados de pronóstico robustos y fiables, manteniendo la interpretabilidad del modelo y la eficiencia computacional.
