

# Añadir teoría básica BBDD (clave-valor, normalización...)
## MySQL

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


## CURSO SQL BÁSICO SQLBOLT.COM

Tabla de condicionales y operadores básicos.

<img src=  "https://github.com/Rubnserrano/apuntes/blob/main/imgs/sql_statements.png?raw=true "/> 

Ejemeplo consulta de bases de datos de películas cuya fecha no esté entre 2000 y 2010
```
SELECT * FROM movies
WHERE YEAR NOT BETWEEN 2000 AND 2010;
```

Otra tabla con más casos:

<img src=  "https://github.com/Rubnserrano/apuntes/blob/main/imgs/otros_sql_statements.png?raw=true "/> 

Un caso en concreto que no he visto en esta página pero que me he encontrado haciendo otros ejercicios es el siguiente caso de uso:
Select all records where the first letter of the `City` is an "a" or a "c" or an "s".
La sentencia sería:

```
SELECT * FROM Customers
WHERE City LIKE '[acs]%';
# si el caso de uso es que no sea ni una a ni una c ni una s sería:
SELECT * FROM Customers
WHERE City LIKE '[!acs]%';
```

De la misma forma si queremos que la condición sea que empiece por cualquier letra de la 'b' a la 's' análogamente se escribiría '[b-s]%'

**Nota**: en un ejercicio de hacker rank me pedía filtrar las ciudades que empezasen por vocal, puse la sentencia
```
....
WHERE City LIKE '[aeiou]%'
```
y me daba error y la correcta era esta:

```
WHERE CITY REGEXP '^[AEIOU]';
```

Para las ciudades que acabaran en vocal:
```
SELECT DISTINCT(CITY) FROM STATION
WHERE CITY  REGEXP '[aeiou]$'   
```

Para las ciudades que empiezan y acaban en vocal:
```
SELECT Distinct(CITY) FROM STATION
WHERE CITY REGEXP '^[aeiou].*[aeiou]$'
```

Otras palabras reservadas en SQL son las siguientes:
- ORDER BY _column_ :  Se pone después de la condición o al final de la secuencia. Puede ir seguido de ASC / DESC. También se pueden ordenar cadenas por la izquierda o la derecha un numero k de caracteres de la forma: ORDER BY left/right(_COLUMNA_, k)
- LIMIT _num limit_ OFFSET _num offset_: el limit está claro, número de resultados queridos. El valor de OFFSET especificará desde donde contar el número de filas.


````
SELECT DISTINCT(Director) FROM movies
ORDER BY Director ASC;
````

Es equivalente a la orden:

```
SELECT Director FROM movies
GROUP BY Director;
```


## Queries multi-tabla con JOINS

La normalización de bases de datos es útil porque minimiza la duplicación de datos en cualquier tabla y permite que los datos de la base de datos crezcan independientemente unos de otros (por ejemplo, los tipos de motores de coche pueden crecer independientemente de cada tipo de coche). Como contrapartida, las consultas se vuelven ligeramente más complejas, ya que tienen que ser capaces de encontrar datos de diferentes partes de la base de datos, y pueden surgir problemas de rendimiento cuando se trabaja con muchas tablas grandes.

Para responder a preguntas sobre una entidad cuyos datos abarcan varias tablas de una base de datos normalizadas, tenemos que aprender a escribir una consulta que pueda combinar todos esos datos y extraer exactamente la información que necesitamos.

Las tablas que comparten información sobre una única entidad necesitan tener una clave primaria que identifique a esa entidad de forma única en toda la base de datos.
Utilizando la cláusula JOIN en una consulta, podemos combinar datos de filas en dos tablas separadas utilizando esta clave única. La primera de las uniones que presentaremos es la INNER JOIN.

### INNER JOIN

 El INNER JOIN es un proceso que empareja filas de la primera tabla y de la segunda tabla que tienen la misma clave (definida por la restricción ON) para crear una fila de resultado con las columnas combinadas de ambas tablas. Una vez unidas las tablas, se aplican las demás cláusulas que hemos aprendido anteriormente.
 
```
SELECT column, another_table_column, … 
FROM mytable 
INNER JOIN another_table ON mytable.id = another_table.id
WHERE condition(s)
ORDER BY column, … ASC/DESC 
LIMIT num_limit OFFSET num_offset;
```

### Otros tipos de JOINS
 Dependiendo de cómo quiera analizar los datos, el INNER JOIN que utilizamos previamente podría no ser suficiente porque la tabla resultante sólo contiene datos que pertenecen a las dos tablas.

Si las dos tablas tienen datos asimétricos, lo que puede ocurrir fácilmente cuando los datos se introducen en diferentes etapas, entonces tendríamos que utilizar un LEFT JOIN, RIGHT JOIN o FULL JOIN en su lugar para asegurarnos de que los datos que necesita no se quedan fuera de los resultados.

<img src=  "https://github.com/Rubnserrano/apuntes/blob/main/imgs/sql_joins.jpg?raw=true "/> 

**Buscar más ejercicios JOINS**

### ALIAS

Podemos hacer combinaciones de nuestras columnas y crear columnas nuevas con la palabra reservada AS.
Un ejemplo sencillo para sumar dos presupuestos y calcular el total en millones de dólares sería:

```
SELECT Year, (Presupuesto1 + Presupuesto2)/1000000 AS Prespuesto_total_millones
....
```

También se usan los alias para simplificar los nombres de columnas y tablas

```
SELECT column AS better_column_name, … 
FROM a_long_widgets_table_name AS mywidgets
INNER JOIN widget_sales 
	ON mywidgets.id = widget_sales.widget_id;
```


### AGG Functions

SQL también soporta el uso de expresiones o funciones de agregación que te permiten resumir la información de un grupo de filas.

De forma general la sintaxis es la siguiente:

```
SELECT AGG_FUNC(_column_or_expression_) AS aggregate_description, … FROM mytable
WHERE constraint_expression;
```

Las funciones más comunes son las descritas en la siguiente tabla:

<img src=  "https://github.com/Rubnserrano/apuntes/blob/main/imgs/agg_functions.png?raw=true "/> 

En lugar de calcular con todas las filas, podemos aplicar estas funciones a grupos de datos. Esto creará tantos resultados como grupos únicos definidos por la cláusula GROUP BY


## HAVING

Nos damos cuenta de que si la orden GROUP BY se ejcuta después del WHERE, ¿cómo podemos filtrar los grupos? Pues con la cláusula HAVING

Sintaxis
```
SELECT group_by_column, AGG_FUNC(_column_expression_) AS aggregate_result_alias, … 
FROM mytable 
WHERE condition 
GROUP BY column 
HAVING group_condition;
```

**Nota**: Si no estamos utilizando una cláusula GROUP BY, un simple WHERE es suficiente.


### Orden de ejecucición de una query.
Después de ver las órdenes básicas, debemos tener claro el orden de estas. Una query completa es de la forma:

```
SELECT DISTINCT column, AGG_FUNC(_column_or_expression_), … 
FROM mytable 
	JOIN another_table 
	ON mytable.column = another_table.column 
	WHERE _constraint_expression_ 
	GROUP BY column HAVING _constraint_expression_ 
	ORDER BY _column_ ASC/DESC 
	LIMIT _count_ OFFSET _COUNT_;`
```

### INSERT

```
INSERT INTO tabla (col1, col2, col3)
VALUES(valor_col1, valor_col2, valor_col3)
```

### UPDATE

```
UPDATE mytable 
SET column = value_or_expr, other_column = another_value_or_expr, …
WHERE condition;
```

### DELETING ROWS

```
DELETE FROM mi_tabla
WHERE condicion
```

**Importante:** Al igual que con la sentencia `UPDATE` anterior, se recomienda ejecutar primero la restricción en una consulta `SELECT` para asegurarse de que se eliminan las filas correctas. Sin una copia de seguridad adecuada o una base de datos de prueba, es muy fácil eliminar datos de forma irrevocable, así que lee siempre tus sentencias Delete dos veces y ejecútalas una.

### CREATING TABLES

```
CREATE TABLE IF NOT EXISTS mytable ( 
	column _DataType_ _TableConstraint_ DEFAULT _default_value_, 
	another_column _DataType_ _TableConstraint_ DEFAULT _default_value_, 
	… 
);
```

Es importante tener en cuenta el schema de la tabla y los tipos de datos que pueden haber. Algunos ejemplos son los siguientes:

<img src=  "https://github.com/Rubnserrano/apuntes/blob/main/imgs/tipos_datos.png?raw=true "/> 

También  es importante saber las constantes de tabla

<img src=  "https://github.com/Rubnserrano/apuntes/blob/main/imgs/tipos_constantes.png?raw=true "/> 

Un ejemplo de consulta de este estilo sería la siguiente:

```
CREATE TABLE movies ( 
	id INTEGER PRIMARY KEY, 
	title TEXT, 
	director TEXT, 
	year INTEGER, 
	length_minutes INTEGER 
);
```

### ALTERING TABLES

Dentro de este apartado tenemos varias opciones: crear o eliminar una columna, renombrar la tabla... entre otros.
La sintaxis para crear una columna usamos la palabra reservada ADD :

```
ALTER TABLE mytable 
ADD column _DataType_ _OptionalTableConstraint_ 
	DEFAULT default_value;
```

Para eliminar una columna se usa DROP:

```
ALTER TABLE mytable 
DROP column_to_be_deleted;
```

Mientras que para renombrar una tabla se usa RENAME TO

```
ALTER TABLE mytable 
RENAME TO new_table_name;
```
### DROPPING TABLES

Por último en este apartado de consultas básicas de SQL tenemos la opción de eliminar tablas.

```
DROP TABLE IF EXISTS mytable;
```

Con toda esta información ya tenemos lo necesario para realizar consultas básicas, pero como todo en la informática, esto se puede complicar infinito.
En el siguiente apartado veremos subqueries y operaciones SET.


### EJERCICIOS HACKERRANK.
En esta página encontraremos un montón de ejercicios para practicar SQL, sin embargo, cuando la cosa se empieza a complicar nos encontramos que las sentencias vistas en el apartado de SQL básico no son suficientes.
Por tanto, conforme me vaya encontrando ejercicios que precisen explicar nuevas cláusulas o formas de escribir las consultas, las iré escribiendo con su explicación.

Uno de los primeros retos que me encuentro es el caso de condicionales, esta vez con un ejercicio de describir que tipo es un triángulo dado la longitud de sus 3 lados en colúmnas A, B, C.
Para resolver esto, es necesario como funcionan los IF en SQL.

### IF STATEMENTS

Si queremos que devuelva algo en función de una condición, en este caso no basta con usar un WHERE. Y para que devuelva lo requerido los condicionales deben encontrarse directamente después del SELECT.
Veamos como sería la sintaxis base y luego el código que resuelve el problema

```
SELECT
	IF(condicion1 AND/OR condicion2 ..., Respuesta,
	IF(....
	IF(
		)) AS nombre_columna
FROM tabla
```

Un aspecto importante que tenemos que destacar es que se pueden anidar ifs como vemos en el ejemplo.

#### Resolución ejercicio.

```
SELECT 
    IF(A + B <= C OR A + C <= B OR B + C <= A, 'Not A Triangle',
    IF(A = B AND B = C, 'Equilateral',
    IF(A = B OR A = C OR B = C, 'Isosceles',
    'Scalene'))) AS Triangle_Type
FROM 
    TRIANGLES;

```

Antes de empezar el apartado de SQL intermedio me gustaría mostrar las diferentes formas de abordar un problema que me he encontrado en hackerrank. Aquí está mi código, que no se adapta a cambios de la bbdd y solo funciona por ensayo y error y todo muy manual porque no sabía otra forma de hacerlo (y lo hice sin usar chatgpt ni mirar documentación):

```
SELECT concat(NAME, IF(Occupation = "Doctor", "(D)", 
         if(Occupation = "Actor", "(A)", 
           if(Occupation = "Singer", "(S)", "(P)"))))  FROM OCCUPATIONS
ORDER BY NAME;

SELECT 'There are a total of', SUM(Occupation = 'Doctor'), 'doctors.' FROM OCCUPATIONS;
SELECT 'There are a total of', SUM(Occupation = 'Actor'), 'actors.' FROM OCCUPATIONS;
SELECT 'There are a total of', SUM(Occupation = 'Singer'), 'singers.' FROM OCCUPATIONS;
SELECT 'There are a total of', SUM(Occupation = 'Professor'), 'professors.' FROM OCCUPATIONS;
```

y la siguiente es la que me he encontrado en foros, que se adapta a cambios en las tablas y seguramente sea varias veces más eficiente que mi consulta:

```
SELECT CONCAT(Name,'(', SUBSTRING(Occupation,1,1),')') FROM OCCUPATIONS ORDER BY Name; 

SELECT CONCAT("There are a total of"," ",count(Occupation)," ",LOWER(Occupation),"s.") FROM OCCUPATIONS GROUP BY Occupation ORDER BY COUNT(Occupation);
```

Con esto lo que quiero abordar es la importancia de saber que cláusulas hay en SQL y que se puede hacer según los requerimientos que nos pidan.

**Repasar substring, concat**
## SQL INTERMEDIO

Bibliografía: hackkerrank (ejercicios tipo leetcode) y sqlbolt.com (teoria + ejercicios)
