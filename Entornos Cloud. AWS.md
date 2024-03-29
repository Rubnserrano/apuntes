# Tema 1: Fundamentos de Cloud Computing

## 1.- Introducción a la Nube

### Conceptos básicos de computación en la nube.
La "nube" o "cloud computing" es un término que engloba un conjunto de servicios de computación, almacenamiento y procesamiento de datos proporcionados a través de Internet por terceros proveedores. En lugar de que los usuarios gestionen sus propios servidores y recursos de TI, estas plataformas ofrecen una variedad de servicios que los usuarios pueden consumir según su demanda. Estos servicios incluyen desde almacenamiento de datos, capacidad de procesamiento, hasta aplicaciones y herramientas de desarrollo, todos accesibles a través de Internet de manera flexible y escalable. La nube permite a los usuarios acceder a tecnología avanzada sin la necesidad de grandes inversiones en infraestructura propia, además de ofrecer beneficios como la flexibilidad, la escalabilidad y el pago por uso. Una de las características más importantes es la escalabilidad automática y bajo demanda. 
En este ámbito, los proveedores más famosos de cloud computing son Amazon con AWS, Microsoft con Azure y Google con GCP. En  esta ocasión nos centraremos en Amazon Web Services, el servicio más usado y demandado.
### Principales servicios AWS
#### EC2 (Elastic Cloud Computing)
Es el servicio de AWS que proporciona capacidad de computación escalable bajo demanda. Es el equivalente a Compute Engine de GCP.
En la práctica AWS nos proporciona 750h de cómputo por mes de algunos tipos de máquina.
#### **¿Cómo creamos la máquina?** 
Le damos a lanzar una instancia en el apartado EC2.
Elegimos un nombre y la versión de la máquina así como el tipo de instancia (elegiremos la opción gratuita)
Crearemos un par de claves (obligatorio para conectarse por ssh)
(Opcional) Permitimos el tráfico http y https por si vamos a usar un servidor web.
Una de las opciones que nos aparecen en la opción de firewall es el uso de grupos de seguridad.
Un **grupo de seguridad** es un conjunto de reglas de firewall para controlar el tráfico de entrada y salida. Se administran en grupos porque normalmente quieres reutilizar las reglas para varios servicios.
Por ejemplo: queremos un servidor web en esta instancia por lo que normalmente vamos a querer un grupo de seguridad donde definamos como se tienen que comportar nuestras instancias con tráfico http (osea por el puerto 80)
#### **¿Cómo conectarse por SSH?**
Para conectarnos por SSH debemos tener a mano el archivo .pem que es nuestro par de claves que creamos anteriormente. Debemos modificar los permisos para sólo el propietario del archivo tenga permiso de lectura. De no ser así, SSH se negará a usar la clave por si ya ha sido comprometida. Los comandos para realizar esto son los siguientes.

En linux:

````
chmod 400 ruta_archivo\claves.pem
````

Mientras que en Windows es algo más complejo, debemos escribir:

```
$path="ruta_archivo\claves.pem"
icacls.exe $path /reset # eliminamos todos los permisos explícitos
icacls.exe $path /GRANT:R "$($env:USERNAME):(R)" # damos permisos de lectura a mi usuario
icacls.exe $path /inheritance:r
```

Para conectarnos finalemente escribimos:

```
ssh -i "ruta_archivo\claves.pem" + ......  #el comando se encuentra en mi máquina -> SSH
```

Y ya estaríamos conectados a la consola de nuestra máquina desde nuestra propia terminal de nuestro dispositivo. Ahora ya podríamos usar nuestra máquina como por ejemplo:

```
mkdir html
cd html
nano index.html #escribimos "hola mundo"
sudo python3 -m http.server 80
```

Si ahora nos vamos a la IP pública de nuestra máquina deberíamos ver el hola mundo (cuidado con seleccionar http y no https)
#### Bases de datos en AWS
Hay diversos servicios que nos proporcionan bases de datos en la nube de Amazon como son:
- DocumentDB: es compatible con MongoDB (pero no es mongoDB en AWS) y es de tipo documetal. 
- DynamoDB: es una bbdd clave-valor noSQL. Ojo: no compatible con MongoDB
- MemoryDB: esta base de datos es a Redis lo que DocumentDB es a MongoDB. 
- RDS: Relational Database Service: base de datos relacional con opciones como MySQL, Postgres, SQLServer, MariaDB...

Los siguientes servicios los veré más rápidamente con el objetivo de hacer un pequeño resumen de estos y cuando haya dado más contenidos entraré más en detalle de cómo utilizar estos servicios en la web y desde el CLI.
#### RDS (Relational Database Service)
Es un servicio web que facilita la configuración, operación y escalado de una base de datos relacional en AWS. Proporciona una capacidad rentable y de tamaño ajustable para una base de datos relacional estándar y se ocupa de las tareas de administración de bases de datos comunes.
Es un servicio totalmente administrado y proporciona una serie de ventajas específicas sobre las implementaciones de bbdd que no están completamente administradas:
- Puede utilizar los productos de base de datos con los que esté familiarizado: MariaDB, SQL Server, MySQL, Oracle y Postgres.
- RDS administra las copias de seguridad, la aplicación de parches de software, la detección automática de errores y recuperación.
- Puede obtener alta disponibilidad con una instancia principal y una instancia secundaria síncrona, con capacidad de conmutación por error en el caso de que surjan problemas.
#### S3 (Simple Storage Service)
Se trata de un servicio de almacenamiento de datos (llamados objetos) que ofrece escalabilidad, disponiblidad de datos y seguridad. Es posible utilizar Amazon S3 para almacenar y proteger cualquier cantidad de datos para diversos casos de uso, tales como datalakes, sitios web, aplicaciones móviles, copias de seguirdad, dispositivos IoT y análisis de big data. Estos datos se guardan en una especie de contenedores llamados buckets.

S3 ofrece varios tipo de almacenamiento diseñados para distintos casos de uso.
<img src=  "https://github.com/Rubnserrano/apuntes/blob/main/imgs/tipos_s3.jpg?raw=true" /> 

#### Glue
AWS Glue es un servicio de integración de datos sin servidor que facilita a los usuarios de análisis descubrir, preparar, migrar e integrar datos de varios orígenes. Puede utilizarlo para análisis, ML y desarrollo de apps.
Con este servicio puede crear, ejecutar y supervisar visualmente pipelines ETL para cargar datos en datalakes. Además, puede buscar y consultar datos ctalogados de forma inmediata mediante Amazon Athena, Amazon EMR y Amazon Redshift.
Glue posee compatibilidad flexible para todas las cargas de trabajo como ETL, ELT y streaming en un sólo servicio. También podemos hacer uso de una interfaz gráfica para crear y editar visualmente los trabajos de Glue con AWS Glue Studio, además de otra herramientra de preparación de datos no code para limpiar y normalizar datos llamada AWS Glue DataBrew.
#### EMR  (tiene versión serverless)
Amazon Elastic MapReduce es una plataforma de clúster administrada que simplifica la ejecución de marcos de trabajo big data, como Hadoop y Spark, para procesar y analizar grandes cantidades de datos. EMR le permite transformar y trasladar grandes cantidades de datos hacia y desde otros alamacenes de datos y bbdd de AWS como S3 y DynamoDB.
Recientemente Amazon oferta EMR Serverless cuya característica que la diferencia de su hermano gemelo es que los usuarios no necesitan tunear, mantener la seguridad y manejar los clusters.  Automáticamente determina los recursos que la app necesita, los obtiene para procesar los jobs y se deshace de los recursos cuando el job ha terminado. Para casos donde las apps necesitan unan respuesta en segundos como análisis de datos interactivo, puedes pre inicializar os recursos que tu app necesita cuando creas la app.
#### Kinesis
Amazon Kinesis Data Streams se usa para recopilar y procesar grandes flujos de registros de datos en tiempo real. Dado que el tiempo de respuesta necesario para la admisión y procesamiento de datos es en tiempo real, el procesamiento suele ser ligero.
Los datos se colocan el flujos de datos de Kinesis, lo que garantiza su durabilidad y elasticidad. El tiempo que transcurre entre el momento en que un registro se inserta en la secuencia y el momento en el que se puede recuperar (retraso put-to-get) es normalmente menor de 1 segundo.
#### Redshift (tiene versión serverless)
Amazon Redshift es un servicio de almacenamiento de datos en la nube proporcionado por Amazon Web Services (AWS). Está diseñado para el análisis de grandes conjuntos de datos y ofrece almacenamiento columnar, escalabilidad masiva, rendimiento rápido e integración con otros servicios de AWS. Además, cuenta con características avanzadas de seguridad. En resumen, Redshift es una solución eficiente y escalable para el análisis de datos en la nube.
#### Athena
AWS Athena es un servicio de consulta interactivo que permite analizar datos almacenados en Amazon S3 utilizando SQL estándar. No requiere administración de infraestructura, ya que simplemente apunta a los datos en S3 y comienza a hacer consultas. Con su integración directa con S3 y el uso de SQL estándar, facilita la adopción y permite a los usuarios escribir consultas sin necesidad de mover o transformar los datos previamente. Además, se factura por la cantidad de datos escaneados por consulta y escala automáticamente para manejar consultas de cualquier tamaño. Es especialmente útil para análisis ad hoc, exploración de datos y casos de uso donde la velocidad de consulta y la flexibilidad son fundamentales.

Amazon Athena es ideal para análisis ad hoc sobre datos en S3 sin la necesidad de administrar infraestructura, mientras que Amazon Redshift es más adecuado para cargas de trabajo de análisis de datos a gran escala que requieren un rendimiento predecible y una administración más avanzada de clústeres.

### Modelos de servicio en la nube: IaaS, PaaS, SaaS.

Cuando nos referimos a **IaaS**, estamos hablando de infraestructuras como servicio. Las empresas contratan la **infraestructura** de hardware a un tercero a cambio de una cuota o alquiler. La contratación de este hardware permite elegir la capacidad de proceso (procesadores), la memoria a utilizar (memoria RAM) y el espacio de almacenamiento (disco duro).
IaaS ofrece también servicios de virtualización como máquinas virtuales, cortafuegos, sistemas de backups o balanceadores de carga.

El servicio **PaaS** ofrece plataformas como servicios. En estas **plataformas** se pueden lanzar aplicaciones como bases de datos, middleware, herramientas de desarrollo, servicios de inteligencia empresarial, etc.
Este tipo de servicios es el ideal para los desarrolladores que sólo quieran centrarse en la implementación y administración de sus aplicaciones. Al no tener que preocuparse por los recursos de hardware y software (sistemas operativo), mejoran su eficacia, centrándose sólo en la parte que les interesa.

El término "SaaS" se refiere a "Software as a Service" (Software como Servicio). Es un modelo de distribución de software donde el proveedor aloja las aplicaciones en la nube y las pone a disposición de los usuarios a través de internet. En lugar de comprar y mantener el software localmente, los usuarios pueden acceder a él a través de sus navegadores web u otros dispositivos conectados a internet mediante una suscripción o un modelo de pago según el uso. Esto permite a las empresas utilizar software sin preocuparse por la instalación, mantenimiento o actualización del mismo, ya que todas esas tareas son responsabilidad del proveedor de SaaS.

## Proyecto iniciación. Spotify ETL
Con objetivo de tener un poco más de soltura en el ecosistema AWS voy a seguir un tutorial para construir mi primer proyecto de prueba.  El link es el siguiente: https://www.youtube.com/watch?v=yIc5a7C8aHs&t=309s

Se trata de un proceso ETL en batch de datos de spotify. Los servicios que se van a usar son los siguientes:
S3, Glue, Athena y Quicksight. De todos estos el único que no está introducido es Quicksight que es una herramienta de visualización.

Lo primero que vamos a hacer es crear un nuevo usuario ya que no es conveniente usar el usuario root para pruebas.

Debemos proporcionar acceso a la consola de administración de AWS y seleccionar la opción 'Quiero crear un usuario de IAM'. Creamos el usuario y en la siguiente página seleccionamos 'Adjuntar políticas directamente'. Agregamos las políticas de los servicios que vamos a usar. En este caso vamos a garantizar acceso completo, pero en un proyecto real añadiríamos permisos más específicos.
En concreto los permisos son los siguientes:

AmazonAthenaFullAccess
AmazonS3FullAccess
AWSGlueConsoleFullAccess
AWSQuicksightAthenaAccess
AWSQuickSightDescribeRDS
IAMUserChangePassword

Guardamos el link inicio de sesión en consola. Cerramos sesión en nuestra cuenta principal e iniciamos desde el link con nuestro nuevo usuario y contraseña.

Creamos un bucket de S3, en este caso el nombre que le pongo es 'proyecto-prueba-spotify' y creamos dos carpetas llamadas staging y datawarehouse. En la vida real, los datos de staging vendrían de una DynamoDB o una instancia de bbdd pero vamos a añadir nuestros datos manualmente. Añadimos a dicha carpeta los datos de albums, artistas y tracks.
Vamos a crear la pipeline con la UI tipo cajitas y para entrar en la interfaz debemos de ir a AWS Glue -> Visual ETL y clickar en el botón naranja Visual ETL.
Debemos crear tres cajas de S3 en la pestaña de Data source y linkar los archivos artist, album y tracks.
Lo siguiente que vamos a hacer es hacer un join entre albums y artistas. Para hacer esto clickamos en la pestaña transform y seleccinamos Join para luego unir artist y albums a la caja de join seleccionando la condición artist_id (del archivo album) = id (del archivo artist).
A continuación creamos otro join que este unido a los datos de tracks y al join personalizado que acabamos de crear. También debemos de poner la condicion id (de tracks) = track_id (del join).
Seguido de esto seleccionamos de la pestaña transform la caja Drop Fields y eliminamos .id ya que está duplicado.
Por último seleccinamos en 'target' un S3 que será el destino cuya ruta será la carpeta datawarehouse.

El código de estas transformaciones es el siguiente:

```
import sys
from awsglue.transforms import *
from awsglue.utils import getResolvedOptions
from pyspark.context import SparkContext
from awsglue.context import GlueContext
from awsglue.job import Job

args = getResolvedOptions(sys.argv, ['JOB_NAME'])
sc = SparkContext()
glueContext = GlueContext(sc)
spark = glueContext.spark_session
job = Job(glueContext)
job.init(args['JOB_NAME'], args)

# Script generated for node artist
artist_node1708776633872 = glueContext.create_dynamic_frame.from_options(format_options={"quoteChar": "\"", "withHeader": True, "separator": ","}, connection_type="s3", format="csv", connection_options={"paths": ["s3://proyecto-prueba-spotify/staging/spotify_artist_data_2023.csv"], "recurse": True}, transformation_ctx="artist_node1708776633872")

# Script generated for node albums
albums_node1708776655388 = glueContext.create_dynamic_frame.from_options(format_options={"quoteChar": "\"", "withHeader": True, "separator": ","}, connection_type="s3", format="csv", connection_options={"paths": ["s3://proyecto-prueba-spotify/staging/spotify-albums_data_2023.csv"], "recurse": True}, transformation_ctx="albums_node1708776655388")

# Script generated for node tracks
tracks_node1708776655833 = glueContext.create_dynamic_frame.from_options(format_options={"quoteChar": "\"", "withHeader": True, "separator": ","}, connection_type="s3", format="csv", connection_options={"paths": ["s3://proyecto-prueba-spotify/staging/spotify_tracks_data_2023.csv"], "recurse": True}, transformation_ctx="tracks_node1708776655833")

# Script generated for node Join
Join_node1708776831367 = Join.apply(frame1=albums_node1708776655388, frame2=artist_node1708776633872, keys1=["artist_id"], keys2=["id"], transformation_ctx="Join_node1708776831367")

# Script generated for node Join with tracks
Joinwithtracks_node1708777165321 = Join.apply(frame1=tracks_node1708776655833, frame2=Join_node1708776831367, keys1=["id"], keys2=["track_id"], transformation_ctx="Joinwithtracks_node1708777165321")

# Script generated for node Drop Fields
DropFields_node1708777326152 = DropFields.apply(frame=Joinwithtracks_node1708777165321, paths=["`.id`"], transformation_ctx="DropFields_node1708777326152")

# Script generated for node destination
destination_node1708777592414 = glueContext.write_dynamic_frame.from_options(frame=DropFields_node1708777326152, connection_type="s3", format="glueparquet", connection_options={"path": "s3://proyecto-prueba-spotify/datawarehouse/", "partitionKeys": []}, format_options={"compression": "snappy"}, transformation_ctx="destination_node1708777592414")

job.commit()
```

<img src=  "https://github.com/Rubnserrano/apuntes/blob/main/imgs/glue_cajitas.png?raw=true "/> 


Podemos guardar el pipeline pero no nos deja correrlo porque necesita un rol de IAM. Para esto, debemos crearlo desde la cuenta raíz. Seleccionamos AWS Service, y en casos de uso Glue. Por último, damos permisos de S3FullAccess y ejecutamos el job.
El siguiente paso es crear un crawler en AWS Glue y seleccionar como data source la carpeta de datawarehouse, seleccionamos el mismo rol de IAM creado anteriormente. Ahora debemos seleccionar una base de datos, pero como no la tenemos la creamos desde el mismo Glue en el apartado Databases. Lo seleccionamos y terminamos de crear el crawler y lo corremos.
Siguiendo el tutorial me daba error al crear el crawler asi que tuve que añadir la política de CloudWatchLogsFullAccess para saber que estaba pasando y AWSGlueConsoleFullAccess para resolver el error.

Creamos un bucket donde guardaremos los outputs de athena. Para hacer consultas primero debemos configurar la salida, que será este bucket y ya podemos hacer queries que se guardarán en las carpetas con la fecha del día de dicho bucket

Ejemplo de resultado de consulta:

<img src=  "https://github.com/Rubnserrano/apuntes/blob/main/imgs/ej_query_spoty.png?raw=true "/> 

El último servicio que utilizaríamos es Quicksight, pero es necesario crearse una cuenta con 1 mes gratis por lo que para no tener que estar pendiente de tener que cancelarla sólo voy a explicar configurar este servicio con nuestros datos. Lo único que hay que hacer es crear un dataset de Athena y seleccionar nuestra tabla datawarehouse.

Ahora volveremos un poco con la teoría basándonos en los cursos que ofrecen Amazon gratuitamente en sus plataformas https://explore.skillbuilder.aws/ y luego realizaremos algunos laboratorios gratuitos que oferta Amazon en https://aws.amazon.com/es/education/awseducate/

En primer lugar voy a realizar el curso 'AWS Cloud Practitioner Essentials' de SkillBuilder con el fin de tener una introducción sólida y combinándolos con los cursos 'Getting started with' de AWS Educate.

## AWS Cloud Practitioner Essentials

#### Diferencia Cloud y On-Premise
La principal diferencia entre la nube y on-premise radica en la ubicación y la gestión de los recursos informáticos: en la nube, los servicios son proporcionados por un proveedor externo a través de Internet, permitiendo una escalabilidad flexible y un modelo de pago por uso, mientras que on-premise implica mantener y gestionar la infraestructura dentro de las instalaciones de la empresa, lo que ofrece un mayor control pero puede ser menos flexible y requerir una inversión inicial significativa.

### Computación en la nube. 
#### Introducción y tipos de instancias.
En este apartado se habla del servicio Elastic Compute Cloud (EC2), que ya introducimos en la introducción a los servicios de AWS. Dependiendo de la necesidad que tengamos EC2 ofrece distintos tipos de máquinas. Por ejemplo, si necesitamos instancias adecuadas para un datawarehouse debemos seleccionar máquinas optimizadas para el almacenamiento. Si en otro caso, necesitamos instancias para bases de datos de high-performance, el tipo de EC2 que deberíamos seleccionar es una optimizada para memoria.
Estos tipos de EC2 no nos aparecen de esta forma, si no que debemos tener en cuenta las especificaciones de RAM, almacenamiento y procesamiento de la lista de máquinas disponibles (esto puede cambiar por región).

#### Pricing
En lo relativo al pricing de EC2, Amazon tiene diferentes opciones de pago:
- On demand: pagas por el tiempo que las instancias están corriendo.
- Planes de ahorro: se ofrecen precios más bajos en EC2 a cambio de un compromiso de uso mínimo por hora. Esto puede ahorrar hasta el 72% del coste.
- Instancias reservadas: es una opción de compra anticipada, diseñada para cargas de trabajo estables o predecibles. Se puede pagar por adelantado, entero o de forma parcial, o sin pago inicial.
La principal diferencia entre estos dos planes es que las instancias reservadas se basan en una capacidad comprometida, mientras que los Saving Plans se basan en un gasto predecible medido en dólares por hora.
- Instancias spot: se te permite usar instancias de EC2 con un descuento de hasta un 90% pero AWS puede reclamar la instancia cuando quiera, dándote un aviso de 2 min.
- Hosts dedicados: servidores físicos exclusivamente reservados para el uso de un cliente en particular.

#### Escalabilidad
Definición: La escalabilidad se refiere a comenzar con solo los recursos necesarios y diseñar la arquitectura de manera que responda automáticamente a cambios en la demanda mediante la expansión o contracción. Como resultado, solo pagas por los recursos que utilizas y no tienes que preocuparte por la falta de capacidad informática para satisfacer las necesidades de tus clientes.

Amazon EC2 ofrece EC2 Auto Scaling que te permite automáticamente añadir o eliminar instancias en respuesta a la demanda. Hay dos enfoques: escalado dinámico, que responde a la demanda cambiante, y escalado predictivo, que programa el número de EC2 que necesitaríamos basado en predicciones. Para escalar más rápido, puedes usar ambos

En términos de escalabilidad, podemos escalar verticalmente (aumentar las especificaciones de nuestras instancias) o de forma horizontal (aumentar el número de instancias).

#### Elastic Load Balancing
Se trata de un servicio de AWS que distribuye automáticamente el tráfico entrante de las apps o servicios en la nube entre múltiples instancias de EC2 o contenedores dentro de una región de AWS. Mejora la disponibilidad y la escalabilidad distribuyendo la carga de manera equitativa entre recursos disponibles

#### Messaging and queuing
La idea básica es que en lugar de que una parte del sistema envíe un mensaje directamente a otra parte, envía el mensaje a una cola. La cola actúa como un buffer temporal que almacena los mensajes hasta que la parte receptora esté lista para procesarlos. Esto significa que si la parte receptora no está disponible en un momento dado, los mensajes permanecen en la cola y no se pierden.
Dos servicios de AWS que ayudan a implementar este tipo de arquitectura son Amazon Simple Queue Service (SQS) y Amazon Simple Notification Service (SNS). SQS es un servicio que proporciona colas de mensajes que puedes utilizar para almacenar mensajes temporales entre diferentes partes de tu sistema. SNS, por otro lado, te permite enviar mensajes a múltiples partes de tu sistema de una sola vez, lo que es útil para enviar notificaciones a diferentes partes de tu aplicación.


# Laboratorios iniciación AWS Educate
## _Lab 'Getting Started with Compute'_

En este laboralatorio introductorio realizamos acciones básicas relacionadas con instancias de EC2. 
Hemos creado y configurado una instancia con Microsoft Windows Server 2019. Entre las configuraciones cabe destacar la selección de una VPC y un grupo de seguridad que no sean la opción default. Así como la asignación de un rol. (Como ya hicimos en la práctica de iniciación, todo el rollo de permisos necesario para el correcto funcionamiento).
Además en las opciones avanzadas hemos seleccionado la opción de 'Termination Protection', una especie de seguro que no te deja terminar la instancia hasta que deselecciones la opción. También hemos pegado código en la caja de 'User data text box'.

```
<powershell>
# Installing web server
Install-WindowsFeature -name Web-Server -IncludeManagementTools
# Getting website code
wget https://aws-tc-largeobjects.s3.us-west-2.amazonaws.com/CUR-TF-100-EDCOMP-1-DEV/lab-01-ec2/code.zip -outfile "C:\Users\Administrator\Downloads\code.zip"
# Unzipping website code
Add-Type -AssemblyName System.IO.Compression.FileSystem
function Unzip
{
    param([string]$zipfile, [string]$outpath)

    [System.IO.Compression.ZipFile]::ExtractToDirectory($zipfile, $outpath)
}
Unzip "C:\Users\Administrator\Downloads\code.zip" "C:\inetpub\"
# Setting Administrator password
$Secure_String_Pwd = ConvertTo-SecureString "P@ssW0rD!" -AsPlainText -Force
$UserAccount = Get-LocalUser -Name "Administrator"
$UserAccount | Set-LocalUser -Password $Secure_String_Pwd
</powershell
```

Este código instala Microsoft Internet Information Services web server, crea una página web simple y configura una contraseña para el usuario Administrador.

También se entra en la parte de monitorización y exploramos una opción que hace captura del escritorio de la máquina. También existe otra opción que se llama Fleet Manager que se encuentra dentro del servicio Systems Manager que te permite conectarte a la máquina e interactuar con toda la interfaz gráfica del so. (Cómo cuando encendemos un pc al uso).

Por último, cambiamos el tipo de máquina y también intentamos apagarla. Para esto debemos desactivar la opción de la que antes hemos hablado de 'Termination protection'.


## _Lab 'Getting Started with Storage'_

Este laboratorio se basa en crear un bucket donde hostearemos una página web estática. Teniendo en cuenta esto, debemos tener las ACL (Listas de control de acceso) habilitadas y también habilitar la opción de configuración de alojamiento de sitios webs estáticos.
Subimos los archivos html, css y javascript necesarios para la web y debemos también hacerlos públicos con ACL mediante la opción Acciones.
Otra función que se trata en este lab es el compartir objetos de forma temporal. Se nos da una url que tiene un tiempo de vida que hemos definido anteriormente.

Por último, creamos una política de buckets para evitar que los archivos sean borrados (mediante código json) y volvemos a subir el archivo html con unos cambios. Observamos el versionamiento de los objetos y que en el bucket queda registrado.

