# Prophet para Análisis de Series Temporales: Apuntes Generales

## Fundamentos del Modelo

Prophet, desarrollado por Meta, es una herramienta avanzada para el pronóstico de series temporales que destaca por su robustez y versatilidad. El modelo se fundamenta en una ecuación aditiva que descompone la serie temporal en componentes esenciales:

```
y(t) = g(t) + s(t) + h(t) + ε(t)
```

En esta ecuación:
- `g(t)` representa la tendencia no lineal que captura cambios a largo plazo
- `s(t)` modela la estacionalidad del sistema
- `h(t)` incorpora efectos de eventos especiales
- `ε(t)` constituye el término de error

## Implementación en Python

La implementación básica requiere la siguiente estructura:

```python
from prophet import Prophet
import pandas as pd

# Configuración del modelo
model = Prophet(
    changepoint_prior_scale=0.05,  
    yearly_seasonality=True,
    seasonality_mode='multiplicative'
)

# Ajuste del modelo
model.fit(df)
```

## Características Avanzadas

### Regresores Adicionales

Prophet permite incorporar variables explicativas externas mediante regresores:

```python
# Incorporación de variables climáticas
model.add_regressor('temperatura')
model.add_regressor('precipitacion')
```

### Validación del Modelo

La evaluación del rendimiento se realiza mediante validación cruzada:

```python
from prophet.diagnostics import cross_validation, performance_metrics

df_cv = cross_validation(model, 
                        initial='730 days', 
                        period='180 days', 
                        horizon='365 days')
metrics = performance_metrics(df_cv)
```

### Optimización de Hiperparámetros

Para encontrar la configuración óptima, se recomienda una búsqueda sistemática:

```python
param_grid = {  
    'changepoint_prior_scale': [0.001, 0.01, 0.1],
    'seasonality_prior_scale': [0.01, 0.1, 1.0],
}

# Evaluación de combinaciones de parámetros
for params in parameter_combinations:
    model = Prophet(**params)
    model.fit(df)
```

## Consideraciones para el Análisis de NDVI

Prophet resulta particularmente efectivo para el análisis de índices de vegetación por:

1. Su capacidad para manejar la estacionalidad multiplicativa, crucial en series NDVI donde los cambios estacionales son proporcionales al nivel base.
2. Su robustez ante datos faltantes, común en observaciones satelitales afectadas por cobertura nubosa.
3. La detección de puntos de cambio que pueden indicar perturbaciones ecológicas significativas.

### Preprocesamiento de Datos NDVI

```python
# Normalización y control de calidad
df['y'] = df['y'].clip(0, 1)  # Restricción al rango válido de NDVI

# Incorporación de incertidumbre
df['y_lower'] = df['y'] - df['ndvi_std']
df['y_upper'] = df['y'] + df['ndvi_std']
```

## Interpretación de Resultados

La descomposición del modelo proporciona insights valiosos:
- La tendencia indica cambios estructurales en la cobertura vegetal
- La estacionalidad revela patrones fenológicos
- Los puntos de cambio pueden identificar eventos disruptivos como incendios o deforestación

La visualización de estos componentes se realiza mediante:

```python
fig = model.plot_components(forecast)
plt.show()
```

Esta implementación permite un análisis riguroso de series temporales de vegetación, combinando la robustez estadística con la interpretabilidad ecológica.
