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

# 2. Procesamiento y EDA
#Se crea un script (por ejemplo, eda_script.py) que realiza:

#Lectura del dataset desde S3.

#Análisis exploratorio (reporte descriptivo, histogramas, frecuencias).

#Transformación de datos (por ejemplo, one-hot encoding).

#El script se genera usando una celda del Notebook:
%%writefile eda_script.py
import argparse
import os
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

def main(input_data, output_data):
    csv_file = os.path.join(input_data, "TestPad_PCB_XYRGB_V2.csv")
    print(f"Leyendo archivo: {csv_file}")
    
    try:
        df = pd.read_csv(csv_file)
    except Exception as e:
        print("Error al leer el CSV:", e)
        return

    pd.set_option('display.max_columns', 500)
    pd.set_option('display.max_rows', 20)
    
    reporte = []
    reporte.append("=== INFORMACIÓN BÁSICA ===")
    reporte.append(f"Forma del dataset: {df.shape}")
    reporte.append(f"Columnas: {df.columns.tolist()}")
    reporte.append("Tipos de datos:\n" + str(df.dtypes))
    reporte.append("Primeras 5 filas:\n" + str(df.head()))
    try:
        reporte.append("Estadísticas descriptivas:\n" + str(df.describe()))
    except Exception as e:
        reporte.append("No se pudieron calcular estadísticas descriptivas: " + str(e))
    
    cat_cols = df.select_dtypes(include=['object']).columns.tolist()
    if cat_cols:
        reporte.append("\n=== FRECUENCIAS DE COLUMNAS CATEGÓRICAS ===")
        for col in cat_cols:
            counts = df[col].value_counts(normalize=True)
            reporte.append(f"Frecuencias para '{col}':\n{counts}")
    
    output_report = os.path.join(output_data, "EDA_Report.txt")
    with open(output_report, "w") as f:
        f.write("\n\n".join(reporte))
    print(f"Reporte guardado en: {output_report}")
    
    num_cols = df.select_dtypes(include=[np.number]).columns.tolist()
    for col in num_cols:
        plt.figure()
        df[col].hist(bins=30)
        plt.title(f"Histograma de {col}")
        plt.xlabel(col)
        plt.ylabel("Frecuencia")
        hist_path = os.path.join(output_data, f"{col}_histogram.png")
        plt.savefig(hist_path)
        plt.close()
        print(f"Histograma guardado para {col} en: {hist_path}")
    
    print("Análisis EDA completado.")

if __name__ == "__main__":
    parser = argparse.ArgumentParser()
    parser.add_argument("--input-data", type=str, default="/opt/ml/processing/input")
    parser.add_argument("--output-data", type=str, default="/opt/ml/processing/output")
    args = parser.parse_args()
    main(args.input_data, args.output_data)

