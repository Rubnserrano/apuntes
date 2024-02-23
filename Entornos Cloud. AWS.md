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

![[tipos_s3.jpg]]
#### Glue
AWS Glue es un servicio de integración de datos sin servidor que facilita a los usuarios de análisis descubrir, preparar, migrar e integrar datos de varios orígenes. Puede utilizarlo para análisis, ML y desarrollo de apps.
Con este servicio puede crear, ejecutar y supervisar visualmente pipelines ETL para cargar datos en datalakes. Además, puede buscar y consultar datos ctalogados de forma inmediata mediante Amazon Athena, Amazon EMR y Amazon Redshift.
Glue posee compatibilidad flexible para todas las cargas de trabajo como ETL, ELT y streaming en un sólo servicio. También podemos hacer uso de una interfaz gráfica para crear y editar visualmente los trabajos de Glue con AWS Glue Studio, además de otra herramientra de preparación de datos no code para limpiar y normalizar datos llamada AWS Glue DataBrew.
#### EMR  (tiene versión serverless)
Amazon Elastic MapReduce es una plataforma de clúster administrada que simplifica la ejecución de marcos de trabajo big data, como Hadoop y Spark, para procesar y analizar grandes cantidades de datos. EMR le permite transformar y trasladar grandes cantidades de datos hacia y desde otros alamacenes de datos y bbdd de AWS como S3 y DynamoDB.
Recientemente Amazon oferta EMR Serverless cuya característica que la diferencia de su hermano gemelo es que los usuarios no necesitan tunear, mantener la seguridad y manejar los clusters.  Automáticamente determina los recursos que la app necesita, los obtiene para procesar los jobs y se deshace de los recursos cuando el job ha terminado. Para casos donde las apps necesitan unan respuesta en segundos como análisis de datos interactivo, puedes pre inicializar os recursos que tu app necesita cuando creas la app.
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

Este modelo de software como infraestructura, aloja el **software** de la empresa, así como sus datos, en servidores externos a la misma, y paga una cuota por su utilización. Cualquier empleado de una empresa podrá acceder desde cualquier lugara las aplicaciones de la empresa sin necesidad de instalarlas en un equipo local. Cuando hablamos de software en la nube estamos hablando de SaaS.
Con un SaaS la preocupación de la empresa será sólo cómo utilizar los programas de software necesarios para su funcionamiento, olvidándose del resto de recursos. El hardware requerido, sistemas operativos, aplicaciones, etc. son provistas por el proveedor del servicio que, además, se encarga de mantenerlas funcionando correctamente y actualizadas.