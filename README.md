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

. ├── 01_data/ # (Ignorado por .gitignore) Contiene los CSV crudos. ├── 02_notebooks/ │ ├── 01_ELT_Polars.ipynb # Notebook 1: Carga CSV, limpia con Polars, carga a BigQuery. │ └── 02_Analysis_BigQuery.ipynb # Notebook 2: Consulta BigQuery con SQL complejo, analiza. ├── 03_powerbi/ │ └── dashboard_logistica.pbix # Archivo de Power BI con los 3 dashboards. ├── .gitignore # Archivo de configuración de GIT (crucial). └── README.md # Esta documentación.

## 5. Fases del Proyecto

### Fase 1: ELT (Polars -> BigQuery)
En el notebook `01_ELT_Polars.ipynb`:
1.  **Extract:** Se lee el CSV de +240,000 filas usando `pl.read_csv()`.
2.  **Transform:** Se limpian y tipifican las columnas. Se usa `pl.col("date").str.to_datetime()` para un parseo de fechas robusto y `pl.select()` para construir el esquema final.
3.  **Load:** Se usa `client.load_table_from_dataframe()` para cargar el DataFrame de Polars (convertido a Pandas) directamente a la tabla `logistica_prod.rutas_clean` en BigQuery, sobrescribiendo (TRUNCATE) los datos anteriores para asegurar la idempotencia.

### Fase 2: Análisis SQL (BigQuery)
En el notebook `02_Analysis_BigQuery.ipynb`, se ejecutan consultas analíticas complejas para responder preguntas de negocio.


Fase 3: Visualización (Power BI)
Se creó un informe de 3 páginas conectado a BigQuery en modo DirectQuery para responder a preguntas clave de la gerencia:

(Página 1: Resumen Ejecutivo) KPIs principales (Tasa de Éxito, Total Paquetes) y tendencia de demanda. (Inserta aquí una captura de pantalla de tu Página 1 del dashboard)

(Página 2: Análisis de Eficiencia) Análisis de tiempos (Conducción vs. Parada), rendimiento de conductores y utilización de vehículos. (Inserta aquí una captura de pantalla de tu Página 2 del dashboard)

(Página 3: Análisis de Factores Externos) Impacto del clima en la velocidad media y tasa de éxito; concentración de incidentes por barrio. (Inserta aquí una captura de pantalla de tu Página 3 del dashboard)

6. Cómo Ejecutar este Proyecto
Clonar el repositorio: git clone https://github.com/tu-usuario/logistics-analytics-pipeline.git

Crear y activar un entorno virtual: python3 -m venv venv && source venv/bin/activate

Instalar las dependencias: pip install jupyterlab polars "polars[rtcompat]" google-cloud-bigquery db-dtypes pyarrow google-cloud-bigquery-storage pandas-gbq

Configurar la autenticación de GCP: gcloud auth login gcloud auth application-default login gcloud config set project logistics-prod-analysis

Colocar el CSV de datos crudos en la carpeta 01_data/.

Ejecutar jupyter-lab y correr los notebooks 01_ y 02_ en orden.

Abrir el archivo .pbix de la carpeta 03_powerbi/ y actualizar las credenciales de BigQuery.
