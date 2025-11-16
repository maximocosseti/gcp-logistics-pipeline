# Pipeline Híbrido de Análisis Logístico: de Datos Crudos a Insights (Polars -> BigQuery -> Power BI)

## 1. Objetivo del Proyecto

Este proyecto demuestra un **pipeline ELT (Extract-Load-Transform) profesional y escalable**, construido para analizar el rendimiento operativo de una empresa de logística. El objetivo es ingerir datos crudos de rutas, modelarlos en un Data Warehouse en la nube y presentar KPIs accionables a la gerencia.

Este portafolio demuestra experiencia práctica en el stack de datos moderno requerido para un rol de Analista Semi-Senior, incluyendo **Python (Polars)**, **GCP (BigQuery)**, **SQL Avanzado** y **Power BI**.

## 2. Arquitectura de la Solución (El "Plan Híbrido")

Para demostrar la capacidad de trabajar con herramientas de nube *reales* (como BigQuery) y al mismo tiempo manejar grandes datasets localmente de forma eficiente (como pide el puesto), se implementó una arquitectura híbrida:

1.  **Entorno DevOps (Local):** Todo el desarrollo se realiza en una **VM de Ubuntu (Linux)**, simulando un entorno de servidor y cumpliendo el requisito de manejo de terminal y archivos.
2.  **Extract-Transform (Local con Polars):** Un script de Python en un **Jupyter Notebook** usa **Polars** para la ingesta y limpieza del CSV. Se eligió Polars sobre Pandas por su motor de **procesamiento paralelo** (Rust) y su capacidad superior para manejar eficientemente datasets que superan la memoria RAM.
3.  **Load (A BigQuery):** El DataFrame limpio de Polars se carga directamente en el **Free-Tier de Google BigQuery** usando la librería cliente de Python (`google-cloud-bigquery`).
4.  **Analysis (Nube con BigQuery SQL):** El análisis pesado (agregaciones, KPIs, rankings) se delega a BigQuery, ejecutando **consultas SQL analíticas complejas** (CTEs y Funciones de Ventana) sobre la tabla en la nube.
5.  **Visualization (Nube con Power BI):** **Power BI** se conecta a la tabla de BigQuery usando **DirectQuery**, asegurando que los dashboards sean escalables y reflejen los datos en tiempo real sin necesidad de importaciones manuales.



## 3. Stack Tecnológico

| Categoría | Herramienta | Razón (Justificación Técnica) |
| :--- | :--- | :--- |
| **Entorno DevOps** | **Ubuntu (Linux)** | Simulación de un entorno de servidor; cumplimiento del requisito de manejo de terminal. |
| **Control de Versiones** | **GIT / GitHub** | Control de código fuente profesional y colaboración. |
| **Análisis (Notebooks)** | **Jupyter Lab** | Entorno estándar de la industria para el análisis exploratorio y desarrollo de pipelines. |
| **Data Cleaning (ETL)** | **Polars (Python)** | **Manejo eficiente de grandes datasets** y **procesamiento paralelo** nativo, superando en rendimiento a Pandas para tareas de ELT. |
| **Data Warehouse (Nube)**| **Google BigQuery** | **Plataforma OLAP serverless** líder. Se utiliza para análisis a gran escala y para demostrar la competencia en GCP. |
| **Análisis de Datos** | **BigQuery SQL** | Dominio de **CTEs (Common Table Expressions)** y **Funciones de Ventana** (Window Functions) para analíticas complejas. |
| **Visualización** | **Power BI** | Herramienta líder de BI. Se usó **DirectQuery** para demostrar la conexión escalable con un Data Warehouse en la nube. |

## 4. Estructura del Repositorio
