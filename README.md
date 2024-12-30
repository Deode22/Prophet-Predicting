# Prophet para Predecir Series Temporales: Apuntes

## Introducción

Este repositorio ofrece una guía práctica sobre Prophet, la biblioteca de Meta para el análisis y predicción de series temporales. Se incluye documentación detallada sobre sus parámetros principales y un caso práctico utilizando datos de NDVI (Índice de Vegetación de Diferencia Normalizada).

## Fundamentos de Prophet

Prophet se basa en un modelo aditivo que descompone las series temporales en sus componentes fundamentales:

```
y(t) = g(t) + s(t) + h(t) + ε(t)
```

Esta descomposición permite analizar por separado:
- La tendencia general (`g(t)`): Captura cambios graduales a largo plazo
- Los patrones estacionales (`s(t)`): Identifica ciclos repetitivos
- Los eventos especiales (`h(t)`): Incorpora efectos de acontecimientos puntuales
- El componente aleatorio (`ε(t)`): Representa las fluctuaciones no explicadas

## Características Destacadas

Prophet destaca por su capacidad para:

1. Manejar datos faltantes sin necesidad de imputación
2. Detectar automáticamente cambios en la tendencia
3. Incorporar diferentes tipos de estacionalidad (diaria, semanal, anual)
4. Ajustar la flexibilidad del modelo mediante parámetros intuitivos

## Contenido del Repositorio

### Documentación
- Guía completa de parámetros de Prophet
- Recomendaciones de ajuste según el tipo de datos
- Interpretación de resultados

### Caso Práctico: Predicción de NDVI
- Preprocesamiento de series temporales
- Configuración y entrenamiento del modelo
- Visualización de predicciones
- Evaluación de resultados

## Requisitos
```python
prophet
pandas
numpy
matplotlib
```

## Contribuciones

Las contribuciones son bienvenidas.

## Licencia
MIT

---
Puedes abrir el notebook en Google Colab
