
# Apuntes Data

Este repositorio contiene apuntes de temas relacionadas con el ámbito de la ingeniería de datos.
Me estoy tomando este repositorio como un archivador de "asignaturas" que forman parte de una especie de cuatrimestre extra con el objetivo de ser un poco más estricto.

Actualmente dichas asignaturas son:

- Arquitecturas Data Engineering y comunicación de datos.
- BBDD y sistemas gestores de BBDD.
- Entornos Cloud. AWS.
- Modern Data Stack.
- Gestión de versiones y despliegue.

Abajo encontraréis las "guías docentes" previsionales creadas con ayuda de ChatGPT con las bases y conocimientos que responden a una serie de skills que me gustaría aprender. 
Nota: seguramente haya cosas algo extrañas e inconcisas. 





# Guías docentes

## Arquitecturas DE y comunicación de datos
### Tema 1: Redes y Protocolos de Comunicación

1. **Funcionamiento de Internet**
    
    - Estructura y funcionamiento de la red de Internet.
    - Conceptos básicos de enrutamiento y direccionamiento IP.
2. **Protocolo TCP/IP**
    
    - Funcionamiento del protocolo TCP/IP.
    - Conceptos de paquetes, puertos y sockets.
3. **Protocolos de Aplicación**
    
    - HTTP y HTTPS: protocolos de transferencia de hipertexto para comunicación web.
    - FTP: protocolo de transferencia de archivos.

### Tema 2: Arquitecturas de Procesamiento de Datos

1. **Introducción a las Arquitecturas de Procesamiento de Datos**
    
    - Batch Processing vs. Stream Processing: diferencias y casos de uso.
    - Arquitecturas lambda y kappa: principios y componentes.
2. **Comparación entre Arquitecturas Lambda y Kappa**
    
    - Ventajas y desventajas de cada arquitectura.
    - Consideraciones de rendimiento, escalabilidad y mantenimiento.

### Tema 3: Microservicios y Arquitecturas de Data Engineering

1. **Microservicios**
    
    - Introducción a la arquitectura de microservicios.
    - Principios de diseño y ventajas frente a monolitos.
2. **Arquitecturas de Data Engineering**
    
    - Componentes clave: ingestion, processing, storage, serving.
    - Herramientas y tecnologías populares: Apache Kafka, Apache Spark, Hadoop, Flink.

### Tema 4: Buenas Prácticas y Proyectos Prácticos

1. **Buenas Prácticas en Arquitecturas de Data Engineering**
    
    - Diseño modular y desacoplado.
    - Monitoreo y gestión de errores.
    - Seguridad y cumplimiento normativo.
2. **Proyectos Prácticos**
    
    - Implementación de un sistema de procesamiento de datos en tiempo real utilizando arquitectura lambda.
    - Desarrollo de un pipeline de datos escalable y tolerante a fallos utilizando microservicios y tecnologías de data engineering.


## BBDD y sistemas gestores BBDD

### Tema 1: Fundamentos de Bases de Datos

1. **Introducción a las Bases de Datos**
    
    - Conceptos básicos: ¿Qué es una base de datos? ¿Por qué son importantes?
    - Tipos de bases de datos: relacionales, NoSQL, NewSQL, etc.
2. **Modelado de Datos**
    
    - Entidades y atributos.
    - Relaciones: uno a uno, uno a muchos, muchos a muchos.
    - Diagramas ER (Entidad-Relación).
3. **SQL (Structured Query Language)**
    
    - DML (Data Manipulation Language): SELECT, INSERT, UPDATE, DELETE.
    - DDL (Data Definition Language): CREATE, ALTER, DROP.
    - Consultas complejas: JOINS, subconsultas, funciones de agregación.
4. **Diseño de Bases de Datos Relacionales**
    
    - Normalización: formas normales (1NF, 2NF, 3NF).
    - Denormalización y optimización.

### Tema 2: Optimización y Rendimiento

1. **Optimización de Queries**
    
    - Estrategias para escribir consultas eficientes.
    - Uso de índices.
    - Perfiles de ejecución y análisis de rendimiento.
2. **Transacciones y Concurrencia**
    
    - ACID properties.
    - Control de concurrencia: bloqueos, transacciones y aislamiento.
3. **Tuning de Bases de Datos**
    
    - Configuración de parámetros del sistema.
    - Estrategias de particionamiento.
    - Monitoreo de rendimiento y ajuste.

### Tema 3: Teoría Avanzada y Tipos Específicos de Bases de Datos

1. **Bases de Datos NoSQL**
    
    - Modelos de datos NoSQL: Documentales, Clave-Valor, Columnares, Grafos.
    - Ejemplos: MongoDB, Redis, Cassandra, Neo4j.
2. **NewSQL y Bases de Datos Distribuidas**
    
    - Características y ventajas.
    - Ejemplos: Google Spanner, CockroachDB.
3. **Bases de Datos en la Nube**
    
    - Conceptos básicos de almacenamiento en la nube.
    - Servicios de bases de datos en la nube: AWS RDS, Azure SQL Database, Google Cloud SQL.

### Tema 4: Proyectos y Práctica

1. **Desarrollo de Proyectos**
    
    - Identificación de requisitos.
    - Diseño e implementación de bases de datos.
    - Integración con aplicaciones y sistemas.
2. **Herramientas y Frameworks**
    
    - Uso de herramientas de administración de bases de datos (MySQL Workbench, pgAdmin).
    - Frameworks de ORM (Object-Relational Mapping): Hibernate, Sequelize.
3. **Proyectos Prácticos**
    
    - Desarrollo de una aplicación con base de datos relacional.
    - Implementación de un sistema de análisis de datos utilizando bases de datos NoSQL.
    - Migración de una base de datos local a la nube.

## Entornos Cloud. AWS.

### Tema 1: Fundamentos de AWS y Cloud Computing

1. **Introducción a la Nube y AWS**
    
    - Conceptos básicos de computación en la nube.
    - Principales servicios de AWS: EC2, S3, RDS, Glue, Athena, Redshift, etc.
    - Modelos de implementación en la nube: IaaS, PaaS, SaaS.
2. **Creación y Configuración de Cuentas en AWS**
    
    - Registro en AWS.
    - Configuración de la interfaz de línea de comandos (CLI) y AWS Management Console.
    - Administración de identidad y acceso: IAM (Identity and Access Management).

### Tema 2: Data Engineering en AWS

1. **Almacenamiento y Gestión de Datos**
    
    - AWS S3: almacenamiento de objetos.
    - AWS Glacier: almacenamiento de datos a largo plazo.
    - AWS EBS: almacenamiento de bloques para instancias EC2.
2. **Procesamiento y Transformación de Datos**
    
    - AWS Glue: servicio de ETL (Extract, Transform, Load).
    - AWS Lambda: ejecución de código sin servidor.
    - AWS Step Functions: coordinación de flujos de trabajo.
3. **Análisis y Visualización de Datos**
    
    - AWS Athena: consulta interactiva de datos en S3.
    - Amazon Redshift: data warehousing.
    - Amazon QuickSight: herramienta de visualización de datos.

### Tema 3: Desarrollo de Pipelines de Datos

1. **Creación de Pipelines de Datos**
    
    - Implementación de pipelines ETL con AWS Glue.
    - Orquestación de tareas con AWS Data Pipeline.
    - Automatización de pipelines con AWS Lambda y Step Functions.
2. **Monitoreo y Optimización de Pipelines**
    
    - Uso de AWS CloudWatch para monitorear recursos.
    - Optimización de costos en AWS.
    - Implementación de buenas prácticas de seguridad y rendimiento.

### Tema 4: Proyectos Prácticos

1. **Desarrollo de Pipelines de Datos**
    
    - Implementación de un pipeline ETL para procesar datos desde S3 hasta Redshift.
    - Creación de un sistema de análisis de datos en tiempo real con AWS Lambda y Kinesis.
    - Desarrollo de un dashboard de visualización utilizando QuickSight.
2. **Integración con Herramientas Externas**
    
    - Integración de herramientas de terceros como Apache Airflow o Jenkins.
    - Implementación de integraciones con servicios externos utilizando APIs de AWS.

## Modern Data Stack

### Tema 1: Fundamentos del Modern Data Stack

1. **Introducción al Modern Data Stack**
    
    - Conceptos básicos: ¿Qué es el Modern Data Stack? ¿Por qué es importante?
    - Visión general de las herramientas principales: dbt, Snowflake, Airflow.
2. **Snowflake**
    
    - Introducción a Snowflake: arquitectura y características clave.
    - Configuración de cuentas y roles.
    - Creación de almacenes de datos y bases de datos virtuales.
3. **dbt (data build tool)**
    
    - Fundamentos de dbt: transformación de datos como código.
    - Configuración de proyectos y modelos.
    - Ejecución de transformaciones y despliegue de resultados.

### Tema 2: Integración y Orquestación

1. **Airflow**
    
    - Introducción a Apache Airflow: orquestación de flujos de datos.
    - Configuración de DAGs (Directed Acyclic Graphs).
    - Programación de tareas y planificación de flujos de trabajo.
2. **Integración de Herramientas**
    
    - Uso de Airflow para orquestar flujos de trabajo de dbt y Snowflake.
    - Configuración de conexiones entre herramientas.

### Tema 3: Optimización y Mejores Prácticas

1. **Optimización de Snowflake**
    
    - Estrategias de almacenamiento y rendimiento.
    - Uso de clústeres y almacenes de tamaño virtual.
2. **Desarrollo Avanzado con dbt**
    
    - Uso de variables y macros en dbt.
    - Implementación de pruebas y documentación en modelos dbt.

### Tema 4: Proyectos Prácticos y Desarrollo Avanzado

1. **Desarrollo de Proyectos de Data Engineering**
    
    - Implementación de un pipeline completo utilizando el Modern Data Stack.
    - Integración con fuentes de datos externas y aplicaciones de análisis.
2. **Desarrollo Avanzado y Personalizado**
    
    - Creación de plugins y extensiones personalizadas para dbt y Airflow.
    - Optimización de flujos de trabajo para grandes volúmenes de datos.


## Gestión de Versiones y Contenerización

### Tema 1: Fundamentos de Control de Versiones y Contenedores

1. **Control de Versiones con Git**
    
    - Introducción a Git: ¿Qué es y por qué se utiliza en desarrollo de software?
    - Configuración básica de Git: instalación, configuración global y creación de repositorios.
    - Conceptos fundamentales: commits, branches, merges y conflictos.
2. **Contenedores con Docker**
    
    - Introducción a Docker: ¿Qué son los contenedores y cómo facilitan el desarrollo y despliegue de aplicaciones?
    - Instalación y configuración de Docker en diferentes sistemas operativos.
    - Uso básico de Docker: creación de contenedores, gestión de imágenes y redes.

### Tema 2: Uso Avanzado de Git y Docker en Proyectos de Data Engineering

1. **Git en Proyectos Colaborativos**
    
    - Buenas prácticas en el uso de Git: ramificación, flujo de trabajo GitFlow.
    - Colaboración en equipos: pull requests, revisión de código y resolución de conflictos.
    - Uso de GitHub, GitLab u otras plataformas de alojamiento de repositorios.
2. **Docker en Proyectos de Data Engineering**
    
    - Dockerfile: creación de imágenes personalizadas para entornos de desarrollo y producción.
    - Docker Compose: gestión de aplicaciones de varios contenedores y definición de servicios.
    - Integración de Docker en flujos de trabajo de CI/CD (Continuous Integration/Continuous Deployment).

### Tema 3: Herramientas Complementarias y Recursos Útiles

1. **Herramientas de Orquestación de Contenedores**
    
    - Introducción a Kubernetes: orquestación de contenedores a escala.
    - Uso de Kubernetes en entornos de desarrollo y producción.
2. **Gestión de Dependencias y Entornos Virtuales**
    
    - Uso de herramientas como pipenv o conda para gestionar dependencias y entornos virtuales en proyectos de Python.
    - Creación y activación de entornos virtuales para aislar dependencias de diferentes proyectos.

### Tema 4: Proyectos Prácticos y Desarrollo Avanzado

1. **Desarrollo de Proyectos con Git y Docker**
    - Creación de un repositorio Git para un proyecto de Data Engineering.
    - Configuración de un entorno de desarrollo con Docker y Docker Compose.
    - Implementación de un flujo de trabajo de CI/CD utilizando Git, Docker y herramientas de integración continua como Jenkins o GitLab CI.
