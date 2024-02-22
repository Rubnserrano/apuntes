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

Para entender todo esto mejor necesitamos entender el modelo OSI.

### Modelo OSI