
## Principales servicios AWS
## EC2 (Elastic Cloud Computing)
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

## Bases de datos en AWS
Hay diversos servicios que nos proporcionan bases de datos en la nube de Amazon como son:
- DocumentDB: es compatible con MongoDB (pero no es mongoDB en AWS) y es de tipo documetal. 
- DynamoDB: es una bbdd clave-valor noSQL. Ojo: no compatible con MongoDB
- MemoryDB: esta base de datos es a Redis lo que DocumentDB es a MongoDB. 
- RDS: Relational Database Server: base de datos relacional con opciones como MySQL, Postgres, SQLServer, MariaDB...

Los siguientes servicios los veré más rápidamente con el objetivo de hacer un pequeño resumen de estos y cuando haya dado más contenidos entraré más en detalle de cómo utilizar estos servicios en la web y desde el CLI.



