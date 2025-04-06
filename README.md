# Processing Job en Amazon SageMaker: EDA, Entrenamiento y Despliegue de Modelo

Este repositorio contiene un Jupyter Notebook que demuestra cómo diseñar y ejecutar un **Processing Job** en Amazon SageMaker. La tarea abarca diferentes fases del ciclo de vida del machine learning, incluyendo:

- **Procesamiento y Feature Engineering:** Realización de un análisis exploratorio de datos (EDA) y transformación del dataset.
- **Entrenamiento del Modelo:** Uso de XGBoost para entrenar un modelo con los datos procesados.
- **Despliegue e Inferencia:** Creación de un endpoint para realizar inferencias en tiempo real con el modelo entrenado.

El proyecto se centra en el dataset **TestPad_PCB_XYRGB_V2.csv**, el cual contiene información de coordenadas, valores de color (R, G, B) y un indicador de píxeles grises. Este dataset fue utilizado originalmente para la recuperación de coordenadas de almohadillas de prueba a partir de imágenes de PCB y presenta aplicaciones en clasificación, detección de anomalías y agrupamiento.

## Tabla de Contenidos

- [Introducción](#introducción)
- [Características](#características)
- [Requisitos](#requisitos)
- [Estructura del Proyecto](#estructura-del-proyecto)
- [Uso](#uso)
- [Flujo de Trabajo](#flujo-de-trabajo)
- [Conclusión](#conclusión)
- [Licencia](#licencia)

## Introducción

Este proyecto demuestra la flexibilidad y potencia de los **Processing Jobs** en Amazon SageMaker, permitiendo automatizar y escalar tareas de preprocesamiento de datos, entrenamiento de modelos y despliegue en un entorno administrado. El Notebook incluye explicaciones detalladas y código comentado para guiar al usuario a través de cada etapa del pipeline de MLOps.

## Características

- **Configuración del Entorno:** Se establece la sesión de SageMaker, se configuran las rutas de entrada y salida en Amazon S3, y se gestionan los permisos necesarios mediante un rol de ejecución.
- **Análisis Exploratorio de Datos (EDA):** Se realiza un análisis completo del dataset, generando reportes y visualizaciones (crosstabs, histogramas, etc.).
- **Feature Engineering:** Se aplican transformaciones como one-hot encoding y la creación de nuevas características.
- **Entrenamiento del Modelo:** Se utiliza XGBoost para entrenar un modelo, configurando hiperparámetros de forma flexible.
- **Despliegue e Inferencia:** Se crea un endpoint para realizar inferencias en tiempo real, demostrando cómo integrar el modelo en un entorno de producción.

## Requisitos

- **AWS Account:** Debes tener una cuenta de AWS con permisos para utilizar Amazon SageMaker y Amazon S3.
- **SageMaker Studio:** Se recomienda utilizar SageMaker Studio para ejecutar el Notebook.
- **Conexión a Internet:** Para descargar dependencias y acceder a los recursos de AWS.
- **Conocimientos básicos de Python y Machine Learning.**

## Estructura del Proyecto

