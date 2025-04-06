
# Pipeline de MLOps con Amazon SageMaker

Este proyecto demuestra cómo construir un pipeline integral de machine learning utilizando Amazon SageMaker. Se abordan diversas fases del ciclo de vida del ML, que incluyen:

- **Procesamiento y EDA:** Transformación y análisis exploratorio del dataset utilizando Processing Jobs.
- **Entrenamiento del Modelo:** Uso de XGBoost para entrenar un modelo a partir de los datos procesados.
- **Despliegue e Inferencia:** Creación de un endpoint para realizar inferencias en tiempo real.

El dataset utilizado, **TestPad_PCB_XYRGB_V2.csv**, contiene información sobre coordenadas (X, Y), valores de color (R, G, B) y un indicador de píxeles grises, con posibles variables derivadas como `avg_color`.

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

Este proyecto ilustra cómo aprovechar Amazon SageMaker para automatizar y escalar tareas del ciclo de vida del machine learning, desde el preprocesamiento de datos hasta el despliegue del modelo en producción. Se utiliza un pipeline modular que separa claramente las fases de procesamiento, entrenamiento y despliegue.

## Características

- **Automatización y Escalabilidad:** Uso de Processing Jobs y Training Jobs para manejar tareas intensivas de datos en la nube.
- **Pipeline Modular:** Separación de las fases de preprocesamiento, entrenamiento e inferencia para facilitar la iteración y el mantenimiento.
- **Integración Continua:** Implementación de un flujo MLOps que permite la validación y el despliegue en tiempo real.
- **Seguridad:** Gestión de permisos a través de roles IAM para controlar el acceso a los recursos de AWS (Amazon S3, SageMaker).

## Requisitos

- Cuenta de AWS con permisos para utilizar Amazon SageMaker y Amazon S3.
- SageMaker Studio o un entorno de Jupyter Notebook para ejecutar el pipeline.
- Python 3.x y las siguientes dependencias: `pandas`, `numpy`, `matplotlib`, `boto3`.

## Estructura del Proyecto


## Guía de Uso

### 1. Configuración del Entorno

Se establece la sesión de SageMaker, se obtiene el rol de ejecución y se definen las rutas de entrada y salida en Amazon S3.

```python
import sagemaker
from sagemaker import get_execution_role
import boto3

# Crear sesión de SageMaker
sagemaker_session = sagemaker.Session()

# Obtener el rol de ejecución
role = get_execution_role()

# Definir bucket y prefijo para organizar los archivos
bucket = "trabajonota2"
prefix = "notebookProcessing"

# Definir rutas en S3
input_data_uri = f"s3://{bucket}/TestPad_PCB_XYRGB_V2.csv"
output_prefix = f"s3://{bucket}/{prefix}/output"

print("S3 Input URI:", input_data_uri)
print("S3 Output Prefix:", output_prefix)
print("IAM Role:", role)



