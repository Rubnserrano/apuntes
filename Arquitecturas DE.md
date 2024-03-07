22/02/24
# Tema 1: Redes y protocolos de comunicación

## Funcionamiento de internet
### Estructura y funcionamiento de la red de internet
Internet es una gran colección de redes que se interconectan entre sí. Una red es un grupo de ordenadores conectados que pueden enviarse datos entre sí.
Gracias a internet, un ordenador puede comunicarse con otro ordenador de una red lejana. Todos los datos que se envían por internet se traducen en pulsos de electricidad o bits que luego interpreta el ordenador receptor.
Internet es un sistema de red distribuido, es decir, que no está centralizado: no depende de ninguna máquina individual.
Cualquier ordenador o hardware que pueda enviar y/o recibir datos de forma adecuada (por ejemplo mediante el uso de protocolos de red correctos) puede formar parte de internet

### ¿Cómo funciona internet?
Caben destacar dos conceptos: paquetes y protocolos.
#### Paquetes
En la red, un paquete es un pequeño segmento de un mensaje más grande. Cada paquete contiene datos e información sobre esos datos. La información sobre el contenido de los paquetes se denomina **cabecera** y va al principio de cada paquete para que la máquina receptora sepa que hacer con él.

Cuando los datos se envían por internet, se dividen primero en paquetes más pequeños que luego se traducen en bits. Los paquetes se enrutan a su destino por diversos dispositivos de red, como enrutadores y conmutadores. Cuando el paquete llega a su destino, la máquina receptora vuelve a ensamblar los paquetes en orden y puede entonces mostrar o utilizar los datos.

Los paquetes se envían por internet mediante una técnica llamada conmutación de paquetes. Los enrutadores y conmutadores intermediarios pueden procesar los paquetes independientemente unos de otros, sin tener en cuenta su origen o destino
#### Protocolos
Conectar dos ordenadores, que pueden tener hardware y software diferentes es todo un reto. Requiere el uso de técnicas de comunicación que puedan entender todos los ordenadores conectados. Este problema se resuelve con protocolos estandarizados.

En la red, un protocolo es una forma estandarizada de realizar determinadas acciones y de dar formato a los datos para que dos o más dispositivos puedan comunicarse y entenderse entre sí.

Hay protocolos para envíar paquetes desde la misma red (ethernet), para enviar paquetes de una red a otra (IP), para garantizar que esos paquetes lleguen en el orden correcto (TCP), y para dar formato a los datos para sitios web y aplicaciones (HTTP).

Además de estos protocolos fundacionales también hay protocolos para el enrutamiento, las pruebas y la encriptación. Y hay alternativas a los protocolos ya comentados como por ejemplo, la transmisión de vídeo suele utilizar UDP en lugar de TCP.


### ¿Qué estructura física hace que internet funcione?
Hay muchos tipos de hardware e infraestructura pero los más importantes son los siguientes:

- Los enrutadores reenvían los paquetes a diferentes redes en función de su destino. Son como los policías de tráfico que aseguran que el tráfico de internet vaya por las rutas (en este caso redes) correctas.
- Los conmutadores (o switches) conectan dispositivos que comparten una misma red. Utilizan la conmutación de paquetes para reenviar los paquetes a los dispositivos correctos. También reciben paquetes de salida de esos dispositivos y los envían al destino correcto.
- Los servidores web son ordenadores especializados de gran potencia que almacenan y sirven contenido (pag web, imágenes, vídeos) a los usuarios, además de alojar aplicaciones y bases de datos. Los servidores también responden a las consultas de DNS y llevan a cabo tareas importantes para que la red siga funcionando. La mayoría se encuentran en grandes centros de datos.


### ¿Qué pasa cuando nos conectamos a una página web?

Cuando entramos a un foro o una web como por ejemplo: https://www.cloudflare.com/es-es/learning/network-layer/how-does-the-internet-work/ (de donde está sacada esta información) se envió pieza a pieza en forma de varios miles de paquetes de datos por internet. 
Estos paquetes viajaron en forma de cables y ondas de radio y a través de conmutadores y enrutadores desde su servidor web hasta tu dispositivo. Tu dispositivo recibió esos paquetes y los envió a tu navegador y él interpretó los datos de los paquetes para mostrar el texto ahora visible.

Los pasos en este proceso son:
- 1.- Consulta de DNS: cuando tu navegador empezó a cargar la web, probablemente primero hizo una consulta de DNS para averiguar la dirección IP de la web que estás viendo.
- 2.- Protocolo de enlace TCP: tu ordenador abrió una conexión con esa IP.
- 3.- Protocolo de enlace TLS: tu navegador también configuró la encriptación entre el servidor web de Cloudflare y tu dispositivo para que los atacantes no puedan leer los paquetes de datos que viajan entre esos dos puntos.
- 4.- Solicitud HTTP: tu navegador solicitó el contenido de la página web.
- 5.- Respuesta HTTP: el servidor de cloudflare transmitió el contenido en forma de código HTML, CSS y JavaScript dividido en una serie de paquetes de datos. Una vez que tu dispositivo recibió los paquetes y verificó que los habías recibido todos, tu navegador interpetó el código HTML, CSS y JS de los paquetes para mostrar el contenido. 
Todo esto en uno o dos segundos.

**Nota**: Los conmutadores operan en la capa de enlace de datos del modelo OSI, mientras que los enrutadores operan en la capa de red.
Bibliografía:  https://www.cloudflare.com/es-es/learning/network-layer/how-does-the-internet-work/

## ChatGTP explica flujo de paquetes
1. Supongamos que estás en una red local junto con otros dispositivos conectados a través de un conmutador.
2. Decides enviar un paquete de datos a un dispositivo que está fuera de tu red local, lo que significa que el paquete debe pasar por un enrutador para llegar a su destino.
3. Cuando envías el paquete, primero pasa a través del conmutador de tu red local. El conmutador examina la dirección MAC de destino en el paquete y lo dirige solo al puerto donde se encuentra el dispositivo de destino en la misma red local. Esto asegura que el tráfico local se mantenga dentro de la red y no cause congestión innecesaria en otros segmentos de la red.
4. Luego, el paquete llega al enrutador. El enrutador examina la dirección IP de destino en el paquete y consulta su tabla de enrutamiento para determinar la mejor ruta hacia la red de destino.
5. Una vez que el enrutador decide la ruta, reenvía el paquete al siguiente salto en esa ruta. Este próximo salto podría ser otro enrutador en la misma red o en una red diferente, dependiendo de la configuración de la red y la ruta óptima hacia el destino final.
6. El paquete sigue su camino a través de la red, pasando por varios enrutadores según sea necesario, hasta que finalmente llega al destino deseado fuera de tu red local.



Una vez realizada esta pequeña introducción que aunque no tenga que ver directamente con muchos de los conceptos prácticos del ámbito data, nunca esta de mal saber, empezamos con la teorñia de procesamiento de datos.

# Tema 2: Arquitecturas de procesamiento de datos
## 1.- Introducción a las Arquitecturas de Procesamiento de datos
### 1.1.- Batch processing vs Stream Processing
Prácticamente todos los datos con los que tratamos son intrínsecamente _streaming_. Los datos se producen casi siempre y se actualizan continuamente en su origen. La _ingestión por lotes_ es una forma especializada y conveniente de procesar este flujo en grandes fragmentos, por ejemplo, manejar un día completo de datos en un sólo lote.

La ingestión por streaming nos permite proporcionar datos a los sistemas posteriores, ya sean otras aplicaciones, bases de datos o sistemas de analítica, a una velocidad continua y en tiempo real. En este caso, _el real-time (o near real-time)_ significa que los datos están disponibles para el sistema posterior poco tiempo después de producirse. 

Los datos por lotes se ingieren en un intervalo de tiempo predeterminado o cuando los datos alcanzan un umbral de tamaño preestablecido.

¿Debo optar por streaming en primer lugar? A pesar de lo atractivo que resulte el enfoque del streaming en primer lugar, hay muchas contrapartidas que hay que entender y tener en cuenta. Estas son algunas preguntas que debemos hacernos para determinar si la ingestión por streaming en lugar de en batch es una opción adecuada:
- Si ingiero los datos en tiempo real, ¿pueden soportar el ritmo de flujo de datos los sistemas de almacenamiento posteriores?
- ¿Necesito una ingestión de datos en milisegundos en tiempo real? ¿O funcionaría un enfoque de microlotes. acumulando e ingeriendo datos, digamos, cada minuto?
- ¿Son fiables y redudantes mi pipeline y mi sistema de streaming si mi infraestructura falla?
Como puede ver, considerar el streaming en primer lugar puede parecer una buena idea, pero no siempre es sencillo, se generan costes y aparecen complejidades adicionales de forma inherente. En resumen, adopte el streaming en tiempo real sólo después de identificar un caso de uso empresarial que justifique las contrapartidas frente al uso de lotes.

### 1.2.- Arquitecturas Lambda y Kappa: principios y componentes.

Entre principios y mediados de la década de 2010, la popularidad del trabajo con datos en streaming se disparó con la aparición de Kafka como cola de mensajes altamente escalable y de frameworks para la analítica en streaming. Estas tecnologías permitieron a las empresas realizar nuevos tipos de analítica y modelos sobre grandes cantidades de datos, agregación y clasificación de usuarios así como recomendaciones de productos. Los ingenieros de datos necesitaban averiguar como conciliar los datos por lotes y en streaming en una única arquitectura. La **arquitectura Lambda** fue una de las primeras respuestas populares a este problema.

### **Arquitectura Lambda**
Esta arquitectura tiene sistemas que operan independientemente unos de otros: lotes, streaming y servicio.
El sistema fuente es idealmente inmutable y solo permite hacer anexiones, enviando los datos a dos destinos para su procesamiento: flujo y lote. El procesamiento en flujo pretende servir los datos con la menor latencia posible en una capa de "velocidad", normalmente una base de datos NoSQL. En la capa de procesamiento por lotes, los datos se procesan y transforman en un sistema como puede ser un data warehouse, creando vistas precalculadas y agregadas de los datos. La capa de servicio proporciona una vista combinada agregando los resultados de las consultas de las dos capas.

<img src=  "https://github.com/Rubnserrano/apuntes/blob/main/imgs/lambda_arch.png?raw=true "/> 

La arquitectura Lambda tiene su parte de desafíos y críticas. La gestión de múltiples sistemas con diferentes bases de código es tan difícil como parece, y se crean sistemas propensos a errores con código y datos extremadamente difíciles de conciliar.

Mencionamos la arquitectura Lambda porque sigue llamando la atención y es popular en los resultados de los motores de búsqueda para la arquitectura de datos. Lambda no es nuestra primera recomendación si está tratando de combinar datos de streaming y por lotes para la analítica. La tecnología y las prácticas han progresado.

A continuación, veamos una reacción a la arquitectura Lambda, la arquitectura **Kappa**


### **Arquitectura Kappa**
Como respuesta a las deficiencias de la arquitectura Lambda, Jay Kreps propuso una alternativa llamada **arquitectura Kappa**. La tesis central es la siguiente: ¿por qué no usar una plataforma de procesamiento de streaming como columna vertebral para toda la gestión de datos, ingestión, almacenamiento y servicio? Esto facilita una verdadera arquitectura basada en eventos. El procesamiento en tiempo real y por lotes se puede aplicar sin problemas a los mismos datos al leer directamente el flujo de eventos en vivo y reproducir grandes fragmentos de datos para el procesamiento por lotes.

<img src=  "https://github.com/Rubnserrano/apuntes/blob/main/imgs/kappa_arch.png?raw=true "/> 

Aunque el artículo original sobre esta arquitectura se publicó en 2014, no hemos visto que se haya producido una adopción generalizada. Puede haber un par de razones para ello. En primer lugar, el streaming en sí mismo sigue siendo un poco misterioso para muchas empresas; es fácil hablar de él, pero más difícil de ejecutar de lo esperado. En segundo lugar, la arquitectura Kappa resulta complicada y cara en la práctica. 


### El modelo Dataflow y la unificación por lotes y streaming
Tanto Lambda como Kappa trataron de abordar las limitaciones del ecosistema Hadoop de la época intentando unir con cinta adhesiva herramientas complicadas que, para empezar, probablemente no encajaban de forma natural. El reto fundamental de unificar los datos por lotes y los de streaming se mantuvo y estas dos arquitecturas proporcionaron inspiración y los cimientos para seguir avanzando.

Uno de los principales problemas de la gestión del procesamiento en batch y en streaming es la unificación de múltiples rutas de código. Aunque la arquitectura Kappa se basa en una capa unificada de colas y almacenamiento, todavía hay que enfrentarse al uso de dferentes herramientos para recoger estadísticas en tiempo real o ejecutar trabajos de agregación de lotes. Hoy en día, los ingenieros tratan de resolver este problema de varias maneras. Google se hizo notar al desarrollar el modelo DataFlow y el framework Apache Beam.

La idea central en el modelo de flujo de datos es ver todos los datos como eventos, ya que la agregación se realiza sobre varios tipos de ventanas. Los flujos de eventos en tiempo real son datos no acotados. Los lotes de datos son simpleente flujos de eventos acotados, y los límites proporcionan una ventana natural. Los ingenieros pueden elegir entre varias ventanas para la agregación en tiempo real, como las deslizantes o las estáticas. El procesamiento en tiempo real y por lotes se realiza en el mismo sistema utilizando un código casi idéntico.

La filosofía de 'lotes como caso especial de streaming' está ahora más extendida. Varios frameworks como Flink y Spark han adoptado un enfoque similar.












Bibliografia: Libro Fundamientos de Ingeniería de Datos, Joe Reis y Matt Housley (O'Reilly)