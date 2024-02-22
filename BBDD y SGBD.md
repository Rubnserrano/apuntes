### MySQL

Para iniciar MySQL en la terminal

```
sql -u ruben -p
```

Listar bases de datos

```
show databases;
use nombre_bbdd;
show tables;
```

### Crear una base de datos mediante línea de comandos

```
CREATE TABLE Clientes (
	ID_Cliente INT NOT NULL PRIMARY KEY AUTO_INCREMENT,
	Nombre VARCHAR(20) NOT NULL,
	Apellido_Paterno VARCHAR(20) NOT NULL,
	Apellido_Materno VARCHAR(20),
	Direccion VARCHAR(50),
	Ciudad VARCHAR(50),
	Estado VARCHAR(2),
	Codigo_Postal VARCHAR(20),
	Correo_electronico VARCHAR(50),
	Edad INT,
	Sexo ENUM('Hombre', 'Mujer', 'Otro'),
	Actividad_Favorita ENUM('Programacion', 'Cocina', 'Ciclismo', 'Correr', 'Ninguna') DEFAULT 'Ninguna',
	Genero_Favorito VARCHAR(50),
	Ocupacion VARCHAR(30)
);
```

Asignamos valores aleatorios generados por chatgpt:

```
INSERT INTO Clientes (Nombre, Apellido_Paterno, Apellido_Materno, Direccion, Ciudad, Estado, Codigo_Postal, Correo_electronico, Edad, Sexo, Actividad_Favorita, Genero_Favorito, Ocupacion) VALUES
('Juan', 'Perez', 'Gomez', 'Calle 123', 'Ciudad de Mexico', 'CM', '12345', 'juan@example.com', 30, 'Hombre', 'Programacion', 'Ciencia Ficcion', 'Ingeniero'),
('Maria', 'Garcia', 'Lopez', 'Avenida 456', 'Guadalajara', 'JA', '54321', 'maria@example.com', 25, 'Mujer', 'Cocina', 'Drama', 'Chef'),
('Luis', 'Rodriguez', 'Fernandez', 'Carrera 789', 'Monterrey', 'NL', '67890', 'luis@example.com', 35, 'Hombre', 'Ciclismo', 'Accion', 'Abogado'),
('Ana', 'Martinez', 'Hernandez', 'Paseo 1011', 'Tijuana', 'BC', '10987', 'ana@example.com', 28, 'Mujer', 'Correr', 'Comedia', 'Doctora'),
('Carlos', 'Lopez', 'Perez', 'Boulevard 1213', 'Puebla', 'PB', '12309', 'carlos@example.com', 40, 'Hombre', 'Ninguna', 'Terror', 'Arquitecto'),
('Laura', 'Hernandez', 'Garcia', 'Camino 1415', 'Queretaro', 'QT', '56789', 'laura@example.com', 22, 'Mujer', 'Cocina', 'Romance', 'Estudiante'),
('Pedro', 'Gomez', 'Martinez', 'Callejon 1617', 'Merida', 'YU', '45678', 'pedro@example.com', 32, 'Hombre', 'Programacion', 'Aventura', 'Programador'),
('Sofia', 'Fernandez', 'Rodriguez', 'Calle 1819', 'Acapulco', 'GR', '87654', 'sofia@example.com', 27, 'Mujer', 'Ciclismo', 'Fantasia', 'Diseñadora'),
('Daniel', 'Gutierrez', 'Sanchez', 'Avenida 2021', 'Cancun', 'QR', '23456', 'daniel@example.com', 29, 'Hombre', 'Correr', 'Suspense', 'Consultor'),
('Fernanda', 'Sanchez', 'Lopez', 'Paseo 2223', 'Veracruz', 'VE', '34567', 'fernanda@example.com', 26, 'Mujer', 'Ninguna', 'Documental', 'Analista');
```

# Consultas

La estrucutra básica de una consulta de SQL se basa en:

````
SELECT col1, col2 FROM tabla
WHERE col3 = resultado AND col4 LIKE 'M%' # que la el dato de la columna 4 empiece por M
LIMIT 10;
````

Algunas de las reglas que hay que seguir:
- Si va a utilizar más de una tabla en la cláusula FROM, todos los campos con el mismo nombre deben de ir precedidos por el nombre de la tabla en donde se vayan a utilizar. Por ejemplo, las tablas Pedidos y Clientes tienen el campo ID_Cliente. Si quisiera llevar a cabo una consulta en estas tablas, necesitaría señalar explícitamente el nombre de la tabla en donde fuera a utilizar el campo ID_Cliente.

````
	SELECT Pedidos.ID_Pedido FROM Pedidos, CLientes
	WHERE Clientes.ID_Cliente = Pedidos.ID_Cliente
````

- Las instrucciones múltiples de una cláusula WHERE deben conectarse mediante el uso de las palabras clave AND u OR.
- Todas las instrucciones SQL deben tener un verbo, una cláusula FROM o INTO

Otra cosa que es común es el uso de alias a una tabla. El código SQL antes escrito se podría simplificar de la siguiente forma

```
	SELECT P.ID_Pedido FROM Pedidos AS P, Clientes AS C
	WHERE P.ID_Cliente = C.ID_Cliente;
```

**Warning**: Tenga cuidado cuando use más de una tabla en los comandos SELECT, UPDATE o DELETE. Asegúrese de que sus uniones están completas y precisas. De lo contrario puede generar una unión cartesiana. _Lo veremos próximamente en el apartado de UNIONS_

Sólo se debe mencionar la tabla si existe alguna ambigüedad en el nombre de los campos. Si tenemos dos tablas con el mismo nombre debe identificar cúal tabla/campo quiere usar. Si el nombre es único en todas las tablas del select no hay que especificarla.
### Función concat() para unir cadenas

```
SELECT concat(Nombre, " ", Primer_apellido, " ", "Segundo Apellido")
AS Nombre_completo, Ciudad, Estado  
FROM Clientes;

 # El AS solo es para el concat, Ciudad y Estado corresponden al select
```

### Resultado de una consulta como consulta

SQL permite utilizar los resultados de una consulta para efectuar otra consulta. Por ejemplo:

```
SELECT * FROM Clientes
WHERE ID_Cliente
IN (SELECT ID_Cliente FROM Pedidos as P, Cia_Envios as E
WHERE E.ID_Cia_Envios = 12
AND E.ID_Cia_Envios = P.ID_Cia_Envios)
```
Selecciona todos los datos de los clientes que cumplen con la segunda consulta, es decir, aquellos que tengan un ID_Cia_Envios igual a 12 en las tablas Pedidos y Cia_Envios

### Guardar consultas en tablas

```
CREATE OR REPLACE consulta1 SELECT * From Pedidos
```

paula es una petardilla