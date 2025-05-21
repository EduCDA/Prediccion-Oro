# Predicción del Precio del Oro con RNN (LSTM)

## Objetivo

Este repositorio tiene como objetivo predecir el precio del oro utilizando una red neuronal recurrente (RNN) con arquitectura LSTM. El modelo se entrena con datos históricos del precio del oro, el índice del dólar (DXY), los bonos del Tesoro a 10 años (TNX) y el ETF TIP (bonos indexados a la inflación). El propósito es explorar la relación entre estos indicadores macroeconómicos y el precio del oro, y construir un modelo capaz de anticipar su evolución a corto plazo.

## Descripción del Proyecto

El flujo de trabajo está estructurado en varias etapas, todas documentadas en el notebook `rnn_gold.ipynb`:

### 1. Descarga y Visualización de Datos
- Se descargan los precios históricos desde Yahoo Finance para:
  - Futuros del oro (`GC=F`)
  - Índice Dólar (`DX-Y.NYB`)
  - Bonos a 10 años (`^TNX`)
  - ETF TIP (`TIP`)
- Se visualizan las series temporales para entender su comportamiento y relaciones.

**Ejemplo de visualización:**
- Gráficas de cada serie (oro, dólar, bonos, TIP) en subplots para comparar tendencias y volatilidad.

### 2. Procesamiento y Feature Engineering
- Se combinan las series temporales en un único DataFrame.
- Se calculan características adicionales:
  - Ratio Oro/Dólar (con lag para evitar data leakage)
  - Inflación implícita (variación mensual del TIP)
  - Tasa real estimada (bonos - inflación)
  - Media móvil simple (SMA 200 días)
  - Bandas de Bollinger (volatilidad histórica)
- Se visualizan el precio del oro junto a la SMA y las bandas de Bollinger para analizar tendencias y volatilidad.

### 3. Normalización y Generación de Secuencias
- Se seleccionan las features relevantes para el modelo.
- Se aplica un escalado robusto para normalizar los datos y evitar el impacto de outliers.
- Se generan secuencias temporales (look-back de 90 días) para alimentar la LSTM, con horizonte de predicción de 5 días.

### 4. División del Dataset
- El dataset se divide temporalmente en:
  - Entrenamiento (2010-2018)
  - Validación (2019-2020)
  - Test (2021-actualidad)
- Se crean generadores de secuencias para cada subconjunto.

### 5. Construcción y Entrenamiento del Modelo LSTM
- Arquitectura:
  - Dos capas LSTM (192 y 96 unidades)
  - Dropout para regularización
  - Capa densa de salida
- Se utiliza EarlyStopping para evitar sobreajuste.
- El modelo se entrena y se visualiza la evolución de la pérdida (MSE) en entrenamiento y validación.

**Ejemplo de visualización:**
- Gráfica de la pérdida (MSE) por época para evaluar el aprendizaje y detectar overfitting.

### 6. Evaluación y Resultados
- Se evalúa el modelo en el conjunto de test.
- Se obtienen predicciones y se invierte la escala para comparar con los valores reales.
- Se visualiza la predicción frente al valor real del precio del oro.
- Se calculan métricas finales: RMSE, MAE y MAPE.

**Ejemplo de visualización:**
- Gráfica comparando la predicción del modelo y el precio real del oro en el test set.

### 7. Guardado del Modelo y Escaladores
- El modelo entrenado se guarda en formato HDF5.
- Se guardan los escaladores utilizados para la variable oro y para el conjunto completo.
- Se almacena metadata relevante sobre el entrenamiento y las features utilizadas.

## Estructura del Repositorio

- `rnn_gold.ipynb`: Notebook principal con todo el flujo de trabajo.
- `modelo_oro/`: Carpeta donde se guardan el modelo entrenado y los escaladores.
- `requirements.txt`: Dependencias necesarias para ejecutar el proyecto.

## Requisitos

- Python 3.8+
- TensorFlow
- Keras
- scikit-learn
- pandas, numpy, matplotlib, seaborn, yfinance, joblib

Instalar dependencias:
```bash
pip install -r requirements.txt
```

## Ejecución

1. Ejecuta el notebook `rnn_gold.ipynb` paso a paso.
2. Al finalizar, encontrarás el modelo y los escaladores en la carpeta `modelo_oro/`.

## Notas
- Las gráficas generadas en el notebook permiten analizar visualmente la calidad de las predicciones y la relación entre las variables macroeconómicas y el precio del oro.
- El modelo puede ser ajustado modificando el look-back, el horizonte de predicción, o añadiendo nuevas features.
