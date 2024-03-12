

# Snowflake

## Introducción a Snowflake. ¿Qué es? ¿Qué soluciona? 

Snowflake es una aplicación SaaS (Software as a Service) basada en el concepto Data Cloud que proporciona una arquitectura de **datos compartidos multiclúster** con altos índices de rendimiento, escalabilidad y simultaneidad.

Snowflake **resuelve el problema de los silos de datos**: es una plataforma que impulsa y proporciona el acceso a una repositorio común de datos en la nube, incluyendo niveles de almacenamiento, procesamiento y servicios globales integrados lógicamente, aunque separados en el espacio físico.

Los ámbitos de aplicación donde Snowflake despliega todas sus virtudes son **Data Warehouse** (con su propio motor SQL), **Data Lake**, ingeniería de datos, ciencia de datos, intercambio de datos y desarrollo de aplicaciones de datos.

Para ubicar mejor en qué consiste Snowflake, podemos decir que es análogo a Synapse (Microsoft Azure), Redshift (Amazon Web Services) o BigQuery (Google Cloud), con la ventaja de que **permite elegir el proveedor de servicios Cloud** que queramos de entre estos tres, así como la región (zona de disponibilidad) de cada uno.
La arquitectura de Snowflake se divide en tres capas de _software_:

1. _Database Storage._
2. _Query Processing._
3. _Cloud Services._

La primera capa es **donde residen los datos**: Snowflake da formato a los datos cuando son subidos a la nube, de modo que su organización queda optimizada en términos de tamaño, metadatos o compresión. Solo pueden ser consultados mediante instrucciones SQL.

La segunda capa consiste en **almacenes virtuales de consultas**. En cada uno de ellos residen varios nodos para trabajar en paralelo en los servicios de la capa siguiente.

La tercera capa contiene los **servicios** encargados de coordinar las actividades. Snowflake las gestiona y se ejecutan sobre las instancias del proveedor de servicios que se haya elegido.
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


## Daily City Metrics Update Sproc (Paso 6 en el esquema)

Durante este paso estaremos creando nuestro segundo Snowpark Python sproc para Snowflake. Este sproc unirá la tabla ORDER_DETAIL con la tabla LOCATION y la tabla HISTORY_DAY para crear una tabla final agregada para el análisis llamada DAILY_CITY_METRICS.

Ejecutamos el siguiente código:

```sql, python
USE ROLE HOL_ROLE;
USE WAREHOUSE HOL_WH;
USE SCHEMA HOL_DB.HOL_SCHEMA;

CREATE OR REPLACE PROCEDURE LOAD_DAILY_CITY_METRICS_SP()
RETURNS VARIANT
LANGUAGE PYTHON
RUNTIME_VERSION = '3.10'
PACKAGES = ('snowflake-snowpark-python')
HANDLER = 'main'
AS
$$

from snowflake.snowpark import Session
import snowflake.snowpark.functions as F

def table_exists(session, schema='', name=''):
    exists = session.sql("SELECT EXISTS (SELECT * FROM INFORMATION_SCHEMA.TABLES WHERE TABLE_SCHEMA = '{}' AND TABLE_NAME = '{}') AS TABLE_EXISTS".format(schema, name)).collect()[0]['TABLE_EXISTS']
    return exists

def main(session: Session) -> str:
    schema_name = "HOL_SCHEMA"
    table_name = "DAILY_CITY_METRICS"
    
    # Define the tables
    order_detail = session.table("ORDER_DETAIL")
    history_day = session.table("FROSTBYTE_WEATHERSOURCE.ONPOINT_ID.HISTORY_DAY")
    location = session.table("LOCATION")
  
    # Join the tables
    order_detail = order_detail.join(location, order_detail['LOCATION_ID'] == location['LOCATION_ID'])
    order_detail = order_detail.join(history_day, (F.builtin("DATE")(order_detail['ORDER_TS']) == history_day['DATE_VALID_STD']) & (location['ISO_COUNTRY_CODE'] == history_day['COUNTRY']) & (location['CITY'] == history_day['CITY_NAME']))

    # Aggregate the data
    final_agg = order_detail.group_by(F.col('DATE_VALID_STD'), F.col('CITY_NAME'), F.col('ISO_COUNTRY_CODE')) \
                        .agg( \
                            F.sum('PRICE').alias('DAILY_SALES_SUM'), \
                            F.avg('AVG_TEMPERATURE_AIR_2M_F').alias("AVG_TEMPERATURE_F"), \
                            F.avg("TOT_PRECIPITATION_IN").alias("AVG_PRECIPITATION_IN"), \
                        ) \
                        .select(F.col("DATE_VALID_STD").alias("DATE"), F.col("CITY_NAME"), F.col("ISO_COUNTRY_CODE").alias("COUNTRY_DESC"), \
                            F.builtin("ZEROIFNULL")(F.col("DAILY_SALES_SUM")).alias("DAILY_SALES"), \
                            F.round(F.col("AVG_TEMPERATURE_F"), 2).alias("AVG_TEMPERATURE_FAHRENHEIT"), \
                            F.round(F.col("AVG_PRECIPITATION_IN"), 2).alias("AVG_PRECIPITATION_INCHES"), \
                        )
                        
    # If the table doesn't exist then create it
    if not table_exists(session, schema=schema_name, name=table_name):
        final_agg.write.mode("overwrite").save_as_table(table_name)
        return f"Successfully created {table_name}"

    # Otherwise update it
    else:
        cols_to_update = {c: final_agg[c] for c in final_agg.schema.names}
        dcm = session.table(f"{schema_name}.{table_name}")
        dcm.merge(final_agg, (dcm['DATE'] == final_agg['DATE']) & (dcm['CITY_NAME'] == final_agg['CITY_NAME']) & (dcm['COUNTRY_DESC'] == final_agg['COUNTRY_DESC']), \
                            [F.when_matched().update(cols_to_update), F.when_not_matched().insert(cols_to_update)])
        return f"Successfully updated {table_name}"
$$;
```

1. **Establecer el rol, almacén y esquema**: El código comienza seleccionando el rol HOL_ROLE, el almacén HOL_WH y el esquema HOL_DB.HOL_SCHEMA. Esto define el contexto en el que se ejecutarán los comandos dentro del procedimiento almacenado.
2. **Crear el procedimiento almacenado**: Define un procedimiento almacenado escrito en Python que calcula y actualiza métricas diarias de ventas por ciudad y país.
3. **Función `table_exists`**: Esta función verifica si una tabla específica ya existe en el esquema especificado. Utiliza una consulta SQL para buscar la tabla en el esquema y devuelve un valor booleano que indica si la tabla existe o no.
4. **Función `main`**: Esta es la función principal del procedimiento almacenado. Realiza las siguientes acciones:
    a. Define el esquema y nombre de la tabla donde se almacenarán las métricas diarias de la ciudad.
    b. Obtiene los datos de tres tablas diferentes: ORDER_DETAIL, FROSTBYTE_WEATHERSOURCE.ONPOINT_ID.HISTORY_DAY y LOCATION.
    c. Une estas tablas utilizando condiciones específicas.
    d. Realiza agregaciones en los datos unidos para calcular las métricas diarias de ventas, temperatura promedio y precipitación promedio por ciudad y país.
    e. Si la tabla de métricas diarias no existe, la crea y guarda los resultados de las agregaciones.
    f. Si la tabla ya existe, actualiza los datos existentes en lugar de sobrescribirlos, utilizando la función merge de Snowflake.
5. **Comando SQL Python**: El código SQL definido como una cadena de texto se ejecutará en Snowflake cuando se invoque el procedimiento almacenado.

Ahora en la UI de Snowflake si vamos a Monitoring -> Query History. Aquí podemos ver todos los logs asociados a nuestra cuenta de Snowflake, sin importar que herramienta o proceso lo haya iniciado.


## Orchestrate jobs (Paso 7 en el esquema)
En este paso estaremos orquestando nuestra nueva pipeline con la herramienta nativa de Snowflake llamada Tasks. Podemos crearlas y deployarlas usando tanto SQL como la API de Task de Python. Esta vez usaremos esta última opción.

Crearemos dos tasks, una por cada procedimiento guardado y las encadenaremos. Desplegaremos las tasks o las correremos para poner en funcionamiento la pipeline.

#### Resumen conceptos importantes orquestación
- **Tareas (Tasks)**: Son la unidad básica de ejecución en Snowflake. Pueden ejecutar una única declaración SQL, llamar a un procedimiento almacenado o cualquier lógica procedural utilizando el scripting de Snowflake. Las tareas forman parte de un Grafo Acíclico Dirigido (DAG).
- **Grafo Acíclico Dirigido (DAG)**: Es una serie de tareas organizadas por sus dependencias, que fluyen en una única dirección. Una tarea se ejecuta solo después de que todas sus tareas predecesoras se hayan ejecutado con éxito.
- **Intervalo de Programación (Schedule Interval)**: Es el tiempo entre ejecuciones sucesivas programadas de una tarea independiente o de la tarea raíz en un DAG.
- **Implementación de un DAG**: Cuando creas un DAG, defines el intervalo de programación, la lógica de transformación en las tareas y las dependencias entre tareas. Sin embargo, es necesario implementar el DAG para que las transformaciones se ejecuten según la programación establecida. Si no se implementa el DAG, no se ejecutarán las transformaciones programadas.
- **Ejecución de un DAG**: Cuando se implementa un DAG, se ejecuta según la programación definida. Cada ejecución programada se denomina "Ejecución de Tarea" (Task Run).

El código para implementar la orquestación es el siguiente:
```python
from datetime import timedelta
#from snowflake.connector import connect
from snowflake.snowpark import Session
from snowflake.snowpark import functions as F
from snowflake.core import Root
from snowflake.core.task import StoredProcedureCall, Task
from snowflake.core.task.dagv1 import DAGOperation, DAG, DAGTask

# Alternative way to create the tasks
def create_tasks_procedurally(session: Session) -> str:
    database_name = "HOL_DB"
    schema_name = "HOL_SCHEMA"
    warehouse_name = "HOL_WH"
    api_root = Root(session)
    schema = api_root.databases[database_name].schemas[schema_name]
    tasks = schema.tasks
    
    # Define the tasks
    task1_entity = Task(
        "LOAD_ORDER_DETAIL_TASK",
        definition="CALL LOAD_EXCEL_WORKSHEET_TO_TABLE_SP(BUILD_SCOPED_FILE_URL(@FROSTBYTE_RAW_STAGE, 'intro/order_detail.xlsx'), 'order_detail', 'ORDER_DETAIL')",
        warehouse=warehouse_name
    )
    task2_entity = Task(
        "LOAD_LOCATION_TASK",
        definition="CALL LOAD_EXCEL_WORKSHEET_TO_TABLE_SP(BUILD_SCOPED_FILE_URL(@FROSTBYTE_RAW_STAGE, 'intro/location.xlsx'), 'location', 'LOCATION')",
        warehouse=warehouse_name
    )
    task3_entity = Task(
        "LOAD_DAILY_CITY_METRICS_TASK",
        definition="CALL LOAD_DAILY_CITY_METRICS_SP()",
        warehouse=warehouse_name
    )
    task2_entity.predecessors = [task1_entity.name]
    task3_entity.predecessors = [task2_entity.name]

    # Create the tasks in Snowflake
    task1 = tasks.create(task1_entity, mode="orReplace")
    task2 = tasks.create(task2_entity, mode="orReplace")
    task3 = tasks.create(task3_entity, mode="orReplace")
  
    # List the tasks in Snowflake
    for t in tasks.iter(like="%task"):
        print(f"Definition of {t.name}: \n\n", t.name, t.definition, sep="", end="\n\n--------------------------\n\n")
    task1.execute()
#    task1.get_current_graphs()
#    task1.suspend()
#    task2.suspend()
#    task3.suspend()
#    task3.delete()
#    task2.delete()
#    task1.delete()

# Create the tasks using the DAG API
def main(session: Session) -> str:
    database_name = "HOL_DB"
    schema_name = "HOL_SCHEMA"
    warehouse_name = "HOL_WH"
    api_root = Root(session)
    schema = api_root.databases[database_name].schemas[schema_name]
    tasks = schema.tasks
    dag_op = DAGOperation(schema)

    # Define the DAG
    dag_name = "HOL_DAG"
    dag = DAG(dag_name, schedule=timedelta(days=1), warehouse=warehouse_name)
    with dag:
        dag_task1 = DAGTask("LOAD_ORDER_DETAIL_TASK", definition="CALL LOAD_EXCEL_WORKSHEET_TO_TABLE_SP(BUILD_SCOPED_FILE_URL(@FROSTBYTE_RAW_STAGE, 'intro/order_detail.xlsx'), 'order_detail', 'ORDER_DETAIL')", warehouse=warehouse_name)
        dag_task2 = DAGTask("LOAD_LOCATION_TASK", definition="CALL LOAD_EXCEL_WORKSHEET_TO_TABLE_SP(BUILD_SCOPED_FILE_URL(@FROSTBYTE_RAW_STAGE, 'intro/location.xlsx'), 'location', 'LOCATION')", warehouse=warehouse_name)
        dag_task3 = DAGTask("LOAD_DAILY_CITY_METRICS_TASK", definition="CALL LOAD_DAILY_CITY_METRICS_SP()", warehouse=warehouse_name)
        dag_task3 >> dag_task1
        dag_task3 >> dag_task2
  
    # Create the DAG in Snowflake
    dag_op.deploy(dag, mode="orreplace")
    dagiter = dag_op.iter_dags(like='hol_dag%')

    for dag_name in dagiter:
        print(dag_name)
    dag_op.run(dag)
    
#    dag_op.delete(dag)
    return f"Successfully created and started the DAG"
    
# For local debugging
# Be aware you may need to type-convert arguments if you add input parameters

if __name__ == '__main__':
    import os, sys
    
    # Add the utils package to our path and import the snowpark_utils function
    current_dir = os.getcwd()
    parent_dir = os.path.dirname(current_dir)
    sys.path.append(parent_dir)
    from utils import snowpark_utils
    session = snowpark_utils.get_snowpark_session()
    if len(sys.argv) > 1:
        print(main(session, *sys.argv[1:]))  # type: ignore
    else:
        print(main(session))  # type: ignore
    session.close()
```

### Breve explicación del código:
dag_task1 carga los datos de order_detail llamando al procedimiento almacenado LOAD_EXCEL_WORKSHEET_TO_TABLE_SP.
dag_task2 carga los datos de localización llamando al procedimiento almacenado LOAD_EXCEL_WORKSHEET_TO_TABLE_SP.
dag_task3 actualiza la tabla DAILY_CITY_METRICS en snowflake llamando al procedimiento almacenado LOAD_DAILY_CITY_METRICS_SP.

**La definición dag_task3 >> dag_task1 significa que dag_task3 depende de dag_task1.**

Una vez creado el DAG y definido el orden de ejecución de las task, lo deployamos con

```python
    dag_op.deploy(dag, mode="orreplace")
```

Además de que se corra periódicamente también podemos ejecutarlo cuando queramos para debuggear con la función

```python
dag_op.run(dag)
```

Snowflake guarda metadatos de casi todo lo que se hace y los tasks no son una excepción. Con esta sentencia SQL podemos monitorizarlas:

```sql
-- Get a list of tasks 
SHOW TASKS; 
-- Task execution history in the past day 
SELECT * 
FROM TABLE(INFORMATION_SCHEMA.TASK_HISTORY( SCHEDULED_TIME_RANGE_START=>DATEADD('DAY',-1,CURRENT_TIMESTAMP()),
										   RESULT_LIMIT => 100)) 
										   ORDER BY SCHEDULED_TIME DESC ;
-- Scheduled task runs 
SELECT 
TIMESTAMPDIFF(SECOND, CURRENT_TIMESTAMP, SCHEDULED_TIME) NEXT_RUN,
SCHEDULED_TIME, NAME, STATE 
FROM TABLE(INFORMATION_SCHEMA.TASK_HISTORY())
WHERE STATE = 'SCHEDULED' 
ORDER BY COMPLETED_TIME DESC;
```

Me he quedado un poco colgado por errores que da el código base, como que no encuentra una función que llamamos con from utils import ..., y parece que esta todo bien definido pero aún así no encuentra la función..





## Tutorial para más adelante Snowflake + dbt + python
https://quickstarts.snowflake.com/guide/data_engineering_with_snowpark_python_and_dbt/index.html?index=..%2F..index#0

## Tutorial Data engineering with snowpark python
https://quickstarts.snowflake.com/guide/data_engineering_pipelines_with_snowpark_python/index.html?index=..%2F..index#9




# DBT

DBT permite a los ingenieros de datos y analistas realizar transformaciones en los datos escribiendo sentencias SQL de tipo SELECT. Internamente, DBT traduce estas sentencias en tablas y en vistas. De esta forma facilita la creación de transformaciones sobre los datos disponibles en el data warehouse

Hay que entender que DBT no realiza movimientos de datos, por lo que no lo deberíamos considerar una herramienta ETL, ELT ni comparar con ellas ya que solamente se encargaría de la «T». Las transformaciones que soporta son las que se pueden hacer con SQL con las que hace pushdown a la tecnología de data warehouse para su ejecución y se configuran con ficheros YAML y plantillas Jinja dinámicas.

**Un modelo en DBT es simplemente una sentencia SELECT que permite transformar datos**. Este modelo debe ser orquestado con otros modelos, y es que DBT permite escribir SQL de manera modular. Para ejecutar, se parte de la creación de un DAG (Grafo Dirigido Acíclico) interno que transforma los datos.

En DBT podemos tener dos modelos que usan la misma subconsulta. En vez de tener que replicarla y mantenerla por duplicado, podemos referenciarla. Lo mismo ocurre a nivel de campos dentro de una consulta.

Así, vemos que los modelos de DBT se basan en el resultado de otros modelos o de la salida de las fuentes de datos. Estas fuentes de datos también se pueden definir con **ficheros de configuración YAML**.

Para enriquecer las consultas se usa Jinja2, un sistema de plantillas para Python. Este motor permite escribir consultas parametrizadas, reutilizar bloques de código y sentencias y ocultar la complejidad subyacente para hacer el código más legible. La modularización permite un mantenimiento del código más sencillo, lo que facilita la labor de los equipos de desarrollo.

La principal ventaja de DBT es que elimina la necesidad de programar en Spark, ya sea con Scala u otro lenguaje. Antes, para conseguir esto, se debía desarrollar un motor propio que gestionaba la generación de código SQL a partir de metadatos, y luego enviarlo a través de JDBC al data warehouse, lo que es mucho más complejo y costoso.