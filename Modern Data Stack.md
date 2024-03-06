

# Snowflake

## Introducción a Snowflake. ¿Qué es? ¿Qué soluciona? 
## Snowflake tutorial #1. Intro to Data Engineering with Snowpark Python. 
## What you will learn

- How to ingest data from an external stage such as an S3 bucket into a Snowflake table
- How to access data from Snowflake Marketplace and use it for your analysis
- How to analyze data and perform data engineering tasks using Snowpark DataFrame API, Python Stored Procedures and more
- How to use open-source Python libraries from curated Snowflake Anaconda channel
- How to create Snowflake Tasks and use the Python Tasks API to schedule data pipelines
- How to use VS Code extension for Snowflake to perform standard snowflake operations from VS Code and Snowsigt UI

<img src=  "https://github.com/Rubnserrano/apuntes/blob/main/imgs/snowflake_project1.png?raw=true "/> 
--------------------------------------------------------------------------------------

Lo primero que debemos hacer es un fork al repositorio https://github.com/Snowflake-Labs/sfguide-data-engineering-with-snowpark-python-intro

También vamos a usar **Github Codespaces** el cual es un servicio de GitHub que permite a los desarrolladores crear y configurar entornos de desarrollo basados en la nube directamente desde sus repositorios de GitHub.

Para empezar debemos estar en nuestro repositorio y clickar en Code -> Codespaces -> Create codespaces on main.

Y esto es lo que va a hacer por nosotros:

- Creating a container for your environment
- Installing Anaconda (miniconda)
- SnowSQL setup
    - Installing SnowSQL
    - Creating a directory and default config file for SnowSQL
- Anaconda setup
    - Creating the Anaconda environment
    - Installing the Snowpark Python library
- VS Code setup
    - Installing VS Code
    - Installing the Snowflake VS Code extension
- Starting a hosted, web-based VS Code editor


## Setup Snowflake
Ahora debemos configurar el archivo ~/.snowsql/config con nuestros datos. Para llegar hasta el le damos a Control + P y escribimos el nombre del archivo.

Para este proyecto vamos a usar la extensión de VS Code de Snowflake para lanzar comandos SQL y crear objetos de Snowflake.

Vamos a ejecutar este código:

```sql
/*-----------------------------------------------------------------------------
Hands-On Lab: Intro to Data Engineering with Snowpark Python
Script:       03_setup_snowflake.sql
Author:       Jeremiah Hansen
Last Updated: 9/26/2023
-----------------------------------------------------------------------------*/

-- SNOWFLAKE ADVANTAGE: Visual Studio Code Snowflake native extension (Git integration)
-- ----------------------------------------------------------------------------
-- Step #1: Accept Anaconda Terms & Conditions
-- ----------------------------------------------------------------------------
-- Step #2: Create the account level objects (ACCOUNTADMIN part)
-- ----------------------------------------------------------------------------

USE ROLE ACCOUNTADMIN;

-- Roles

SET MY_USER = CURRENT_USER();

CREATE OR REPLACE ROLE HOL_ROLE;

GRANT ROLE HOL_ROLE TO ROLE SYSADMIN;

GRANT ROLE HOL_ROLE TO USER IDENTIFIER($MY_USER);

  

GRANT EXECUTE TASK ON ACCOUNT TO ROLE HOL_ROLE;

GRANT MONITOR EXECUTION ON ACCOUNT TO ROLE HOL_ROLE;

GRANT IMPORTED PRIVILEGES ON DATABASE SNOWFLAKE TO ROLE HOL_ROLE;

  
-- Databases

CREATE OR REPLACE DATABASE HOL_DB;

GRANT OWNERSHIP ON DATABASE HOL_DB TO ROLE HOL_ROLE;


-- Warehouses

CREATE OR REPLACE WAREHOUSE HOL_WH WAREHOUSE_SIZE = XSMALL, AUTO_SUSPEND = 300, AUTO_RESUME= TRUE;

GRANT OWNERSHIP ON WAREHOUSE HOL_WH TO ROLE HOL_ROLE;
-- ----------------------------------------------------------------------------

-- Step #3: Create the database level objects

-- ----------------------------------------------------------------------------

USE ROLE HOL_ROLE;

USE WAREHOUSE HOL_WH;

USE DATABASE HOL_DB;


-- Schemas

CREATE OR REPLACE SCHEMA HOL_SCHEMA;

  
-- External Frostbyte objects

USE SCHEMA HOL_SCHEMA;

CREATE OR REPLACE STAGE FROSTBYTE_RAW_STAGE

    URL = 's3://sfquickstarts/data-engineering-with-snowpark-python/'

;
```

Vamos a ir explicando este código paso a paso:

```sql
SET MY_USER = CURRENT_USER();
CREATE OR REPLACE ROLE HOL_ROLE; 
GRANT ROLE HOL_ROLE TO ROLE SYSADMIN;
GRANT ROLE HOL_ROLE TO USER IDENTIFIER($MY_USER);
````
Primero establecemos la variable MY_USER al usuario actual. 
Creamos el rol HOL_ROLE y le damos los mismos permisos que SYSADMIN para luego otorgarle este rol a mi usuario actual.

```sql
GRANT EXECUTE TASK ON ACCOUNT TO ROLE HOL_ROLE;
GRANT MONITOR EXECUTION ON ACCOUNT TO ROLE HOL_ROLE;
GRANT IMPORTED PRIVILEGES ON DATABASE SNOWFLAKE TO ROLE HOL_ROLE;
```
Le damos permisos para correr tasks, monitorizarlas y para importar privilegios en la base de datos.

```sql
CREATE OR REPLACE DATABASE HOL_DB;
GRANT OWNERSHIP ON DATABASE HOL_DB TO ROLE HOL_ROLE;
```
Creamos una nueva base de datos y le asignamos la propiedad de la bbdd al rol que hemos creado anteriormente (recordemos que nuestro usuario está asociado a este rol).

```sql
CREATE OR REPLACE WAREHOUSE HOL_WH WAREHOUSE_SIZE = XSMALL, AUTO_SUSPEND = 300, AUTO_RESUME= TRUE;
GRANT OWNERSHIP ON WAREHOUSE HOL_WH TO ROLE HOL_ROLE;
```
Creamos un datawarehouse de tamaño XSMALL que se autosuspenda a los 5 mins (300segs) y activamos la reanudación automática. También le otorgamos la propiedad del warehouse HOL_WH al rol HOL_ROLE.

```sql
USE ROLE HOL_ROLE;
USE WAREHOUSE HOL_WH;
USE DATABASE HOL_DB;
```
Inicializamos todos los objetos que hemos creado anteriormente, por lo tanto establecemos el contexto de ejecución.

```
CREATE OR REPLACE SCHEMA HOL_SCHEMA;
USE SCHEMA HOL_SCHEMA;
CREATE OR REPLACE STAGE FROSTBYTE_RAW_STAGE
    URL = 's3://sfquickstarts/data-engineering-with-snowpark-python/'
```
Creamos un esquema y lo inicializamos.
Creamos una **etapa** (stage) externa en Snowflake. Una etapa es un punto de acceso a un servicio de almacenamiento externo, como un bucket de Amazon S3. En este caso, se está creando una etapa llamada `FROSTBYTE_RAW_STAGE` en el esquema `HOL_SCHEMA` que apunta a la URL especificada, que es un bucket de Amazon S3.

Ahora ejecutamos el código, a mi me dio el error No active Snowflake session y para resolverlo hay que pinchar en el icono de snowflake de la barra de la izquierda e iniciar sesión con tus credenciales.

## Load data (paso 4 en el esquema)
Aquí va la parte donde subimos los datos pero en vez de eso vamos a acceder a ellos mediante el marketplace de Snowflake.

***¿Qúe es el Marketplace de Snowflake?***
Snowflake Marketplace proporciona visibilidad a una amplia variedad de conjuntos de datos de administradores de datos de terceros que amplían el acceso a los puntos de datos utilizados para transformar los procesos empresariales. Snowflake Marketplace también elimina la necesidad de integrar y modelar datos al proporcionar un acceso seguro a conjuntos de datos totalmente mantenidos por el proveedor de datos.

Nos dirigimos a la siguiente url: https://app.snowflake.com/marketplace/listing/GZSOZ1LLEL/weather-source-llc-weather-source-llc-frostbyte?search=Weather%20Source%20LLC%3A%20frostbyte y pulsamos en GET.
Modificamos el nombre de bbdd a FROSTBYTE_WEATHERSOURCE y el rol al que hemos creado antes: HOL_ROLE

A continuación ejecutamos el siguiente código:

```
USE ROLE HOL_ROLE;

USE WAREHOUSE HOL_WH;

SELECT * FROM FROSTBYTE_WEATHERSOURCE.ONPOINT_ID.POSTAL_CODES LIMIT 100;
```


## Load Location and Order Detail (Paso 5 en el esquema)
En este paso vamos a cargar dos archivos excel: location.xlsx y order_detail.xlsx de nuestro S3 a Snowflake. Para hacer esto ejecutamos el siguiente código:

```sql
USE ROLE HOL_ROLE;
USE WAREHOUSE HOL_WH;
USE SCHEMA HOL_DB.HOL_SCHEMA;

LIST @FROSTBYTE_RAW_STAGE/intro;

CREATE OR REPLACE PROCEDURE LOAD_EXCEL_WORKSHEET_TO_TABLE_SP(file_path string, worksheet_name string, target_table string)
RETURNS VARIANT
LANGUAGE PYTHON
RUNTIME_VERSION = '3.10'
PACKAGES = ('snowflake-snowpark-python', 'pandas', 'openpyxl')
HANDLER = 'main'
AS
$$
from snowflake.snowpark.files import SnowflakeFile
from openpyxl import load_workbook
import pandas as pd

def main(session, file_path, worksheet_name, target_table):
 with SnowflakeFile.open(file_path, 'rb') as f:
     workbook = load_workbook(f)
     sheet = workbook.get_sheet_by_name(worksheet_name)
     data = sheet.values
     # Get the first line in file as a header line
     columns = next(data)[0:]
     # Create a DataFrame based on the second and subsequent lines of data
     df = pd.DataFrame(data, columns=columns)
     df2 = session.create_dataframe(df)
     df2.write.mode("overwrite").save_as_table(target_table)
 return True
$$;

CALL LOAD_EXCEL_WORKSHEET_TO_TABLE_SP(BUILD_SCOPED_FILE_URL(@FROSTBYTE_RAW_STAGE, 'intro/order_detail.xlsx'), 'order_detail', 'ORDER_DETAIL');
CALL LOAD_EXCEL_WORKSHEET_TO_TABLE_SP(BUILD_SCOPED_FILE_URL(@FROSTBYTE_RAW_STAGE, 'intro/location.xlsx'), 'location', 'LOCATION');
DESCRIBE TABLE ORDER_DETAIL;
SELECT * FROM ORDER_DETAIL;
DESCRIBE TABLE LOCATION;
SELECT * FROM LOCATION;
```

Este código hace lo siguiente:
1. **Establecer el rol, almacén y esquema**: Se utiliza el rol HOL_ROLE, el almacén HOL_WH y el esquema HOL_DB.HOL_SCHEMA para realizar todas las operaciones siguientes. Esto define el contexto en el que se ejecutarán los comandos restantes.
2. **Listar archivos en la etapa**: Utiliza el comando `LIST` para mostrar los archivos disponibles en la ubicación @FROSTBYTE_RAW_STAGE/intro. Esto puede ayudar a verificar que los archivos necesarios están presentes antes de proceder con la carga.
3. **Crear el procedimiento almacenado**: Define un procedimiento almacenado llamado `LOAD_EXCEL_WORKSHEET_TO_TABLE_SP` que carga datos desde una hoja de cálculo Excel a una tabla en Snowflake. El procedimiento toma tres parámetros: `file_path` (ruta del archivo), `worksheet_name` (nombre de la hoja de cálculo) y `target_table` (nombre de la tabla de destino).
4. **Llamar al procedimiento almacenado**: Llama al procedimiento `LOAD_EXCEL_WORKSHEET_TO_TABLE_SP` dos veces, una vez para cada archivo Excel en la etapa. El primer archivo es "order_detail.xlsx" y se carga en la tabla "ORDER_DETAIL", mientras que el segundo archivo es "location.xlsx" y se carga en la tabla "LOCATION".
5. **Describir y seleccionar datos de las tablas**: Utiliza los comandos `DESCRIBE` y `SELECT` para verificar que los datos se hayan cargado correctamente en las tablas ORDER_DETAIL y LOCATION, respectivamente.


# Daily City Metrics Update Sproc

Durante este paso estaremos creando nuestro segundo Snowpark Python sproc para Snowflake. Este sproc unirá la tabla ORDER_DETAIL con la tabla LOCATION y la tabla HISTORY_DAY para crear una tabla final agregada para el análisis llamada DAILY_CITY_METRICS.
## Tutorial para más adelante Snowflake + dbt + python
https://quickstarts.snowflake.com/guide/data_engineering_with_snowpark_python_and_dbt/index.html?index=..%2F..index#0

## Tutorial Data engineering with snowpark python
https://quickstarts.snowflake.com/guide/data_engineering_pipelines_with_snowpark_python/index.html?index=..%2F..index#9