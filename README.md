# Pipeline de MLOps con Amazon SageMaker

Este proyecto demuestra cómo construir un pipeline integral de machine learning utilizando Amazon SageMaker. A través de distintas fases —procesamiento, análisis exploratorio de datos (EDA), entrenamiento, despliegue e inferencia— se resalta la flexibilidad y escalabilidad que ofrece SageMaker para automatizar tareas críticas y optimizar recursos en la nube.

## Tabla de Contenidos

1. [Introducción](#introducción)
2. [Características](#características)
3. [Requisitos](#requisitos)
4. [Estructura del Proyecto](#estructura-del-proyecto)
5. [Guía de Uso](#guía-de-uso)
    - [5.1 Configuración del Entorno](#51-configuración-del-entorno)
    - [5.2 Procesamiento y EDA](#52-procesamiento-y-eda)
    - [5.3 Verificación de la Salida en S3](#53-verificación-de-la-salida-en-s3)
    - [5.4 Entrenamiento del Modelo](#54-entrenamiento-del-modelo)
    - [5.5 Despliegue e Inferencia](#55-despliegue-e-inferencia)
6. [Conclusión](#conclusión)
7. [Licencia](#licencia)

## Introducción

En el entorno actual de machine learning, es fundamental contar con pipelines que permitan automatizar y escalar el procesamiento de datos y la implementación de modelos. Este proyecto se centra en el uso de Amazon SageMaker para construir un flujo de trabajo completo, que abarca desde el preprocesamiento y análisis exploratorio de datos (EDA) hasta el entrenamiento, despliegue e inferencia de modelos. La solución propuesta demuestra cómo integrar distintas fases del ciclo de vida del ML en un pipeline modular, facilitando la iteración rápida y la optimización continua.

## Características

- **Automatización:** Utiliza Processing Jobs y Training Jobs para automatizar tareas complejas, liberando recursos y tiempo.
- **Escalabilidad:** Se aprovecha la infraestructura administrada de SageMaker, permitiendo escalar procesos de procesamiento y entrenamiento según la demanda.
- **Modularidad:** Cada fase del pipeline está claramente definida y separada, lo que facilita la iteración y mejora independiente de cada componente.
- **Integración Continua:** El pipeline permite la implementación de modelos en producción mediante endpoints para inferencias en tiempo real.
- **Seguridad:** La configuración de roles y permisos en AWS garantiza un acceso seguro y controlado a los recursos, como Amazon S3.

## Requisitos

- **Cuenta de AWS** con permisos para utilizar Amazon SageMaker y Amazon S3.
- **SageMaker Studio** o un entorno compatible con Jupyter Notebook.
- **Python 3.x** junto con librerías como `pandas`, `numpy`, `matplotlib` y `boto3`.

## Estructura del Proyecto

La estructura del repositorio es la siguiente:

. ├── SageMaker_Processing_Job_Estudio.ipynb # Notebook principal que describe y ejecuta el pipeline completo ├── eda_script.py # Script de Python para procesamiento y EDA (generado desde el Notebook) └── README.md # Este archivo de documentación

## Guía de Uso

### 5.1 Configuración del Entorno

En esta etapa se establece la sesión de SageMaker, se obtiene el rol de ejecución y se definen las rutas de entrada y salida en Amazon S3. Estos pasos son críticos para asegurar que todas las fases del pipeline puedan acceder a los recursos necesarios de forma segura y escalable.

### 5.2 Procesamiento y EDA

Se utiliza un Processing Job en SageMaker para ejecutar un script de Python que realiza el análisis exploratorio y la transformación del dataset. El script (guardado como `eda_script.py`) realiza las siguientes tareas:

- Lee el dataset desde la ubicación en S3.
- Realiza un análisis exploratorio, generando un reporte con información básica, estadísticas descriptivas y frecuencias de las columnas categóricas.
- Genera visualizaciones como histogramas para las columnas numéricas.
- Guarda el reporte y las visualizaciones en la ruta de salida en S3.

### 5.3 Verificación de la Salida en S3

Una vez finalizado el Processing Job, se verifica que los resultados (reportes, histogramas, etc.) se hayan subido correctamente a la ubicación de salida en S3. Este paso es fundamental para confirmar que el procesamiento se ha realizado correctamente antes de pasar a la fase de entrenamiento del modelo.

### 5.4 Entrenamiento del Modelo

Con los datos procesados disponibles en S3 (por ejemplo, los splits de entrenamiento y validación), se configura un Training Job utilizando XGBoost. Durante esta fase se:
 
- Define la ubicación de los datos de entrenamiento y validación.
- Configura el modelo y sus hiperparámetros.
- Ejecuta el Training Job para entrenar el modelo de forma escalable.

Es importante que los datos de entrenamiento se encuentren en el formato requerido por XGBoost (por ejemplo, sin encabezado y con solo valores numéricos).

### 5.5 Despliegue e Inferencia

Después del entrenamiento, el modelo se despliega en un endpoint de SageMaker para realizar inferencias en tiempo real. Esta fase incluye:

- Desplegar el modelo en un endpoint.
- Configurar el serializador para que el endpoint reciba datos en formato CSV.
- Realizar inferencias de ejemplo para validar que el modelo responda correctamente.
- Eliminar el endpoint una vez validadas las inferencias, para evitar costos adicionales.

## Conclusión

Este proyecto demuestra cómo Amazon SageMaker permite construir un pipeline de MLOps integral y escalable. Se han implementado las siguientes fases:

- **Configuración del Entorno:** Se estableció un entorno seguro y escalable mediante la configuración de sesiones, roles y rutas en S3.
- **Procesamiento y EDA:** Se ejecutó un Processing Job que transformó y analizó el dataset, generando reportes y visualizaciones que respaldan decisiones informadas.
- **Entrenamiento del Modelo:** Se entrenó un modelo utilizando XGBoost, optimizando hiperparámetros y aprovechando la infraestructura administrada para procesar grandes volúmenes de datos.
- **Despliegue e Inferencia:** El modelo se desplegó en un endpoint, permitiendo realizar inferencias en tiempo real y demostrando la integración continua en un entorno de producción.

Este enfoque modular y automatizado facilita la iteración rápida, la mejora continua y la escalabilidad de las soluciones de machine learning en la nube, permitiendo a los equipos de data science centrarse en la optimización y el desarrollo de modelos de alto rendimiento.

## Licencia

Este proyecto está bajo la [MIT License](LICENSE).
