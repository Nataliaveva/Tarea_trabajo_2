# Pipeline de MLOps con Amazon SageMaker

Este proyecto demuestra cómo construir un pipeline de machine learning utilizando **Processing Jobs** en Amazon SageMaker. Se abordan diversas fases del ciclo de vida del ML, desde el preprocesamiento y análisis exploratorio de datos (EDA), pasando por el entrenamiento de un modelo con XGBoost, hasta el despliegue e inferencia en producción.

El objetivo es resaltar la flexibilidad y potencia de SageMaker para automatizar tareas críticas, optimizando recursos y permitiendo una integración fluida de las distintas etapas del proceso.

## Tabla de Contenidos

- [Introducción](#introducción)
- [Características](#características)
- [Requisitos](#requisitos)
- [Estructura del Proyecto](#estructura-del-proyecto)
- [Guía de Uso](#guía-de-uso)
  - [1. Configuración del Entorno](#1-configuración-del-entorno)
  - [2. Procesamiento y EDA](#2-procesamiento-y-eda)
  - [3. Verificación de la Salida en S3](#3-verificación-de-la-salida-en-s3)
  - [4. Entrenamiento del Modelo](#4-entrenamiento-del-modelo)
  - [5. Despliegue e Inferencia](#5-despliegue-e-inferencia)
- [Conclusión](#conclusión)
- [Licencia](#licencia)

## Introducción

En este proyecto se construye un pipeline completo que abarca:
- **Preprocesamiento y análisis exploratorio de datos (EDA):** Se transforma un dataset que contiene información sobre coordenadas y valores de color (R, G, B) de píxeles, junto con un indicador de píxeles grises.
- **Entrenamiento del Modelo:** Se utiliza XGBoost para entrenar un modelo a partir de los datos preprocesados.
- **Despliegue e Inferencia:** El modelo se despliega en un endpoint de SageMaker, demostrando la capacidad de realizar inferencias en tiempo real.

Este flujo de trabajo es aplicable a diversos escenarios del machine learning, permitiendo automatizar y escalar procesos críticos en un entorno administrado en la nube.

## Características

- **Automatización:** Uso de Processing Jobs y Training Jobs para automatizar tareas de preprocesamiento, entrenamiento y despliegue.
- **Escalabilidad:** Capacidad de procesar grandes volúmenes de datos en un entorno administrado, separando el desarrollo (notebook) de la ejecución en recursos optimizados.
- **Integración:** Pipeline modular que integra procesamiento, análisis, entrenamiento y despliegue, facilitando la iteración rápida y la mejora continua del modelo.
- **Seguridad:** Gestión de permisos a través de roles IAM para asegurar el acceso controlado a los recursos de AWS, como Amazon S3.

## Requisitos

- **Cuenta de AWS** con permisos para usar Amazon SageMaker y Amazon S3.
- **SageMaker Studio** o Jupyter Notebook para ejecutar el pipeline.
- **Python 3.x** y dependencias como `pandas`, `numpy`, `matplotlib`, y `boto3`.

## Estructura del Proyecto


## Guía de Uso

### 1. Configuración del Entorno

Se establece la sesión de SageMaker, se obtiene el rol de ejecución y se definen las rutas en S3 para los datos de entrada y salida.  
Ejemplo:

```python
import sagemaker
from sagemaker import get_execution_role
import boto3

# Crear sesión de SageMaker
sagemaker_session = sagemaker.Session()

# Obtener el rol de ejecución
role = get_execution_role()

# Definir bucket y prefijo
bucket = "trabajonota2"
prefix = "notebookProcessing"

# Rutas en S3
input_data_uri = f"s3://{bucket}/TestPad_PCB_XYRGB_V2.csv"
output_prefix = f"s3://{bucket}/{prefix}/output"

print("S3 Input URI:", input_data_uri)
print("S3 Output Prefix:", output_prefix)
print("IAM Role:", role)

