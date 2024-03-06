

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

```sql
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

````sql
SELECT col1, col2 FROM tabla
WHERE col3 = resultado AND col4 LIKE 'M%' # que la el dato de la columna 4 empiece por M
LIMIT 10;
````

Algunas de las reglas que hay que seguir:
- Si va a utilizar más de una tabla en la cláusula FROM, todos los campos con el mismo nombre deben de ir precedidos por el nombre de la tabla en donde se vayan a utilizar. Por ejemplo, las tablas Pedidos y Clientes tienen el campo ID_Cliente. Si quisiera llevar a cabo una consulta en estas tablas, necesitaría señalar explícitamente el nombre de la tabla en donde fuera a utilizar el campo ID_Cliente.

````sql
	SELECT Pedidos.ID_Pedido FROM Pedidos, CLientes
	WHERE Clientes.ID_Cliente = Pedidos.ID_Cliente
````

- Las instrucciones múltiples de una cláusula WHERE deben conectarse mediante el uso de las palabras clave AND u OR.
- Todas las instrucciones SQL deben tener un verbo, una cláusula FROM o INTO

Otra cosa que es común es el uso de alias a una tabla. El código SQL antes escrito se podría simplificar de la siguiente forma

```sql
	SELECT P.ID_Pedido FROM Pedidos AS P, Clientes AS C
	WHERE P.ID_Cliente = C.ID_Cliente;
```

**Warning**: Tenga cuidado cuando use más de una tabla en los comandos SELECT, UPDATE o DELETE. Asegúrese de que sus uniones están completas y precisas. De lo contrario puede generar una unión cartesiana. _Lo veremos próximamente en el apartado de UNIONS_

Sólo se debe mencionar la tabla si existe alguna ambigüedad en el nombre de los campos. Si tenemos dos tablas con el mismo nombre debe identificar cúal tabla/campo quiere usar. Si el nombre es único en todas las tablas del select no hay que especificarla.
### Función concat() para unir cadenas

```sql
SELECT concat(Nombre, " ", Primer_apellido, " ", "Segundo Apellido")
AS Nombre_completo, Ciudad, Estado  
FROM Clientes;

 # El AS solo es para el concat, Ciudad y Estado corresponden al select
```

### Resultado de una consulta como consulta

SQL permite utilizar los resultados de una consulta para efectuar otra consulta. Por ejemplo:

```sql
SELECT * FROM Clientes
WHERE ID_Cliente
IN (SELECT ID_Cliente FROM Pedidos as P, Cia_Envios as E
WHERE E.ID_Cia_Envios = 12
AND E.ID_Cia_Envios = P.ID_Cia_Envios)
```
Selecciona todos los datos de los clientes que cumplen con la segunda consulta, es decir, aquellos que tengan un ID_Cia_Envios igual a 12 en las tablas Pedidos y Cia_Envios

### Guardar consultas en tablas

```sql
CREATE OR REPLACE consulta1 SELECT * From Pedidos
```


## CURSO SQL BÁSICO SQLBOLT.COM

Tabla de condicionales y operadores básicos.

<img src=  "https://github.com/Rubnserrano/apuntes/blob/main/imgs/sql_statements.png?raw=true "/> 

Ejemeplo consulta de bases de datos de películas cuya fecha no esté entre 2000 y 2010
```sql
SELECT * FROM movies
WHERE YEAR NOT BETWEEN 2000 AND 2010;
```

Otra tabla con más casos:

<img src=  "https://github.com/Rubnserrano/apuntes/blob/main/imgs/otros_sql_statements.png?raw=true "/> 

Un caso en concreto que no he visto en esta página pero que me he encontrado haciendo otros ejercicios es el siguiente caso de uso:
Select all records where the first letter of the `City` is an "a" or a "c" or an "s".
La sentencia sería:

```sql
SELECT * FROM Customers
WHERE City LIKE '[acs]%';
# si el caso de uso es que no sea ni una a ni una c ni una s sería:
SELECT * FROM Customers
WHERE City LIKE '[!acs]%';
```

De la misma forma si queremos que la condición sea que empiece por cualquier letra de la 'b' a la 's' análogamente se escribiría '[b-s]%'

**Nota**: en un ejercicio de hacker rank me pedía filtrar las ciudades que empezasen por vocal, puse la sentencia
```sql
...
WHERE City LIKE '[aeiou]%'
```
y me daba error y la correcta era esta:

```sql
WHERE CITY REGEXP '^[AEIOU]';
```

Para las ciudades que acabaran en vocal:
```sql
SELECT DISTINCT(CITY) FROM STATION
WHERE CITY  REGEXP '[aeiou]$'   
```

Para las ciudades que empiezan y acaban en vocal:
```sql
SELECT Distinct(CITY) FROM STATION
WHERE CITY REGEXP '^[aeiou].*[aeiou]$'
```

Otras palabras reservadas en SQL son las siguientes:
- ORDER BY _column_ :  Se pone después de la condición o al final de la secuencia. Puede ir seguido de ASC / DESC. También se pueden ordenar cadenas por la izquierda o la derecha un numero k de caracteres de la forma: ORDER BY left/right(_COLUMNA_, k)
- LIMIT _num limit_ OFFSET _num offset_: el limit está claro, número de resultados queridos. El valor de OFFSET especificará desde donde contar el número de filas.


````sql
SELECT DISTINCT(Director) FROM movies
ORDER BY Director ASC;
````

Es equivalente a la orden:

```sql
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
 
```sql
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

```sql
SELECT Year, (Presupuesto1 + Presupuesto2)/1000000 AS Prespuesto_total_millones
....
```

También se usan los alias para simplificar los nombres de columnas y tablas

```sql
SELECT column AS better_column_name, … 
FROM a_long_widgets_table_name AS mywidgets
INNER JOIN widget_sales 
	ON mywidgets.id = widget_sales.widget_id;
```


### AGG Functions

SQL también soporta el uso de expresiones o funciones de agregación que te permiten resumir la información de un grupo de filas.

De forma general la sintaxis es la siguiente:

```sql
SELECT AGG_FUNC(_column_or_expression_) AS aggregate_description, … FROM mytable
WHERE constraint_expression;
```

Las funciones más comunes son las descritas en la siguiente tabla:

<img src=  "https://github.com/Rubnserrano/apuntes/blob/main/imgs/agg_functions.png?raw=true "/> 

En lugar de calcular con todas las filas, podemos aplicar estas funciones a grupos de datos. Esto creará tantos resultados como grupos únicos definidos por la cláusula GROUP BY


## HAVING

Nos damos cuenta de que si la orden GROUP BY se ejcuta después del WHERE, ¿cómo podemos filtrar los grupos? Pues con la cláusula HAVING

Sintaxis
```sql
SELECT group_by_column, AGG_FUNC(_column_expression_) AS aggregate_result_alias, … 
FROM mytable 
WHERE condition 
GROUP BY column 
HAVING group_condition;
```

**Nota**: Si no estamos utilizando una cláusula GROUP BY, un simple WHERE es suficiente.


### Orden de ejecucición de una query.
Después de ver las órdenes básicas, debemos tener claro el orden de estas. Una query completa es de la forma:

```sql
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

```sql
INSERT INTO tabla (col1, col2, col3)
VALUES(valor_col1, valor_col2, valor_col3)
```

### UPDATE

```sql
UPDATE mytable 
SET column = value_or_expr, other_column = another_value_or_expr, …
WHERE condition;
```

### DELETING ROWS

```sql
DELETE FROM mi_tabla
WHERE condicion
```

**Importante:** Al igual que con la sentencia `UPDATE` anterior, se recomienda ejecutar primero la restricción en una consulta `SELECT` para asegurarse de que se eliminan las filas correctas. Sin una copia de seguridad adecuada o una base de datos de prueba, es muy fácil eliminar datos de forma irrevocable, así que lee siempre tus sentencias Delete dos veces y ejecútalas una.

### CREATING TABLES

```sql
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

```sql
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

```sql
ALTER TABLE mytable 
ADD column _DataType_ _OptionalTableConstraint_ 
	DEFAULT default_value;
```

Para eliminar una columna se usa DROP:

```sql
ALTER TABLE mytable 
DROP column_to_be_deleted;
```

Mientras que para renombrar una tabla se usa RENAME TO

```sql
ALTER TABLE mytable 
RENAME TO new_table_name;
```
### DROPPING TABLES

Por último en este apartado de consultas básicas de SQL tenemos la opción de eliminar tablas.

```sql
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

```sql
SELECT
	IF(condicion1 AND/OR condicion2 ..., Respuesta,
	IF(....
	IF(
		)) AS nombre_columna
FROM tabla
```

Un aspecto importante que tenemos que destacar es que se pueden anidar ifs como vemos en el ejemplo.

#### Resolución ejercicio.

```sql
SELECT 
    IF(A + B <= C OR A + C <= B OR B + C <= A, 'Not A Triangle',
    IF(A = B AND B = C, 'Equilateral',
    IF(A = B OR A = C OR B = C, 'Isosceles',
    'Scalene'))) AS Triangle_Type
FROM 
    TRIANGLES;

```

Antes de empezar el apartado de SQL intermedio me gustaría mostrar las diferentes formas de abordar un problema que me he encontrado en hackerrank. Aquí está mi código, que no se adapta a cambios de la bbdd y solo funciona por ensayo y error y todo muy manual porque no sabía otra forma de hacerlo (y lo hice sin usar chatgpt ni mirar documentación):

```sql
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

```sql
SELECT CONCAT(Name,'(', SUBSTRING(Occupation,1,1),')') FROM OCCUPATIONS ORDER BY Name; 

SELECT CONCAT("There are a total of"," ",count(Occupation)," ",LOWER(Occupation),"s.") FROM OCCUPATIONS GROUP BY Occupation ORDER BY COUNT(Occupation);
```

Con esto lo que quiero abordar es la importancia de saber que cláusulas hay en SQL y que se puede hacer según los requerimientos que nos pidan.

**Repasar substring, concat**
## SQL INTERMEDIO

#### SUBQUERIES
Hay situaciones en las que para obtener la información que nosotros necesitamos no nos basta de los resultados de una única consulta, por lo que tendremos que realizar consultas de otras consultas.

Se puede hacer referencia a una subconsulta en cualquier lugar donde se pueda hacer referencia a una tabla normal.  Dentro de un FROM puedes realizar JOINS con subconsultas, dentro de restricciones WHERE o HAVING puedes probar expresiones con los resultados de subconsultas e incluso en SELECTs, que te permitan devolver datos directamente de la subconsulta.
**Importante:** Cada subconsulta debe estar completamente entre paréntesis para establecer la jerarquía adecuada.

_Sub consultas correlacionadas_
La consulta interna hace referencia a una columna o alias de la consulta externa y depende de ella. A diferencia de las subconsultas anteriores, cada una de las consultas debe ejecutarse para cada una de las filas de la consulta externa, ya que la consulta interna depende de la fila de la consulta externa actual.

Ejemplo:

```
En lugar de la lista anterior solo de Asociados de Ventas, imagine si tiene una lista general de Empleados, sus departamentos (ingeniería, ventas, etc.), ingresos y salario. Esta vez, está buscando en toda la empresa los empleados que se desempeñan peor que el promedio en su departamento.

Para cada empleado, deberá calcular su costo en relación con los ingresos promedio generados por todas las personas de su departamento. Para tomar el promedio del departamento, la subconsulta necesitará saber en qué departamento se encuentra cada empleado:
```

```sql
SELECT * FROM employees 
WHERE salary > 
	(SELECT AVG(revenue_generated) 
	FROM employees AS dept_employees 
	WHERE dept_employees.department = employees.department);
```

_Pruebas de existencia_
Cuando hablamos del uso del WHERE anteriormente en las consultas básicas, usamos el operador IN para verificar si el valor existía en una lista fija de valores. En consultas más complejas, esto se puede ampliar mediante subconsultas para probar si existe un valor en una lista dinámica de valores.

```sql
SELECT *, … 
FROM mytable 
WHERE column 
	IN/NOT IN (SELECT another_column 
			   FROM another_table);
```


#### UNIONS, INTERSECTIONS AND EXCEPTIONS

Cuando estamos trabajando con varias tablas, las cláusulas UNION y UNION ALL te permiten unir resultados de dos consultas asumiendo que tengan el mismo número de columnas, orden y tipo de datos. Si usamos el UNION sin ALL, las filas duplicadas entre tablas se eliminarán del resultado.

```sql
SELECT column, another_column 
FROM mytable 
	UNION / UNION ALL / INTERSECT / EXCEPT 
SELECT other_column, yet_another_column 
FROM another_table 
ORDER BY column DESC 
LIMIT n;
```

En cuanto al orden, las sentencias UNION se colocan antes de ORDER BY y LIMIT. No es muy común utilizar UNIONs pero si tenemos datos de diferentes tablas que no pueden ser juntadas y procesadas con JOINS puede ser una alternativa.
Similar a los UNIONS, el operador INTERSECT se asegurará de que solo se devuelvan las filas que sean idénticas en ambos conjuntos. El EXCEPT se asegurará de que solo se devuelvan las filas del primer conjunto de resultados que no estén en el segundo. Esto significa que el EXCEPT depende del orden de las consultas, como LEFT JOIN y RIGHT JOIN.
INTERSECT y EXCEPT también descartan filas duplicadas después de sus respectivas operaciones aunque en algunas bbdd también se admiten INTERSECT ALL y EXCEPT ALL.


## Ejercicio MURDER MISTERY
Con el objetivo de afianzar los conocimientos con un juego propongo esta especie de práctica cuyo objetivo es encontrar a un asesino a través de consultas SQL.

El schema de la base de datos es el siguiente:

<img src=  "https://github.com/Rubnserrano/apuntes/blob/main/imgs/schema.png?raw=true "/> 

De primeras sabemos que hubo un asesinato en algun lugar de SQL City el dia 15 de enero del 2018.
Por tanto buscamos en el reporte de escenas del crimen si hay algo registrado con esta consulta:

```sql
select * from crime_scene_report
where date = 20180115 and city = 'SQL City' and type = 'murder';
```

El resultado que nos arroja son los dos testigos que registraron: 
- Una persona llamada Annabel que vive en algun lado de "Franklin Ave"
- Una persona que vive en la última casa de "Northwestern Dr"

Buscamos a las personas que se llamen Annabel + algo y que vivan en Franklin Ave.
```sql
select * from person            
where address_street_name = "Franklin Ave"
AND name LIKE '%Annabel%';
```
y obtenemos los siguientes datos:

<img src=  "https://github.com/Rubnserrano/apuntes/blob/main/imgs/sospechoso1.png?raw=true "/> 

Para utilizar las subqueries, voy a buscar en la tabla interview todo lo que haya filtrando por el id de Annable de la siguiente forma:

```sql
select * from interview     
where person_id = (SELECT id from person
						 where address_street_name = "Franklin Ave"
						 AND name LIKE '%Annabel%');
```

cuyo resultado es el siguiente: 
**'I saw the murder happen, and I recognized the killer from my gym when I was working out last week on January the 9th.'** 
Obtenemos el primer sospechoso, Joe Germuska, el cual estuvo a la vez en el gimnasio que Annabel el día 9/01/2018. 

```sql
select membership_id, name, id,person_id, check_in_date, check_in_time, check_out_time from get_fit_now_check_in
join get_fit_now_member on get_fit_now_check_in.membership_id = get_fit_now_member.id
where check_in_date = 20180109 and check_in_time BETWEEN 1600 AND 1700;
```

<img src=  "https://github.com/Rubnserrano/apuntes/blob/main/imgs/sospechoso_gym.png?raw=true "/> 

Y con su id filtramos por más tablas, buscando más pistas. 
Obtenemos más información suya si buscamos en la tabla persons, de donde obtenemos su número de carnet de conducir, su domiciolio y su ssn (que parece ser el número de la seguridad social o algo del estilo)

<img src=  "https://github.com/Rubnserrano/apuntes/blob/main/imgs/datos_joe.png?raw=true "/> 

No parece que le hayan interrogado todavía ni que haya publicado nada en facebook.  
Buscamos en la tabla de licencia de conducir más datos suyos como su edad y apariencia física. Sin embargo, pese a tener número de licencia, no se encuentra en la base de datos (o no soy capaz de encontrarlo). Tampoco vive con nadie más, asi que de momento no podemos avanzar más.

Investigamos al otro testigo e intentamos localizarlo con la siguiente query:
```sql
select * from person
where address_street_name = 'Northwestern Dr'
order by address_number desc
limit 1;
```

<img src=  "https://github.com/Rubnserrano/apuntes/blob/main/imgs/sospechoso2.png?raw=true "/> 

y esa persona comentó en la entrevista lo siguiente:
**I heard a gunshot and then saw a man run out. He had a "Get Fit Now Gym" bag. The membership number on the bag started with "48Z". Only gold members have those bags. The man got into a car with a plate that included "H42W". **

¡Ajá! Así que con esto sabemos podremos localizar su coche y observar por qué no aparece en la tabla de conductores. Lo hacemos con la siguiente query:

```sql
select * from drivers_license
WHERE plate_number LIKE "H42W%";
```

cuyo resultado es:

<img src=  "https://github.com/Rubnserrano/apuntes/blob/main/imgs/coche_sospechoso.png?raw=true "/> 

Por tanto el sospechoso se subió a un Toyota Prius de matrícula H42W0X cuyo titular es una mujer rubia de 21 años y de ojos azules con id de licencia 183449. Vamos a investigar más sobre esta chica con la siguiente query anidada:

```sql
select person.id, name, license_id, address_number, address_street_name, ssn from person 
join drivers_license on drivers_license.id = person.license_id
where person.license_id = (select id from drivers_license
WHERE plate_number LIKE "H42W%")
```

cuyo resultado es:

<img src=  "https://github.com/Rubnserrano/apuntes/blob/main/imgs/vecina_sospechoso.png?raw=true "/> 

y nos damos cuenta de que es Maxine Whitely, la vecina del sospechoso Joe Germuska. Lo siguiente que haremos será buscar en facebook por si publicó algo esa noche. No parece que haya más información sobre Maxine y Joe.
Por tanto, vamos a buscar con quien más coincidieron las testigos esa noche con la siguiente query:

```sql
select * from person
where id in (select person_id from facebook_event_checkin
where event_name = 'The Funky Grooves Tour');
```

con un tal 'Jeremy Bowers', vamos a buscar más información sobre el en otras tablas.

```sql
select * from get_fit_now_member where
person_id = (select id from person
where id in (select person_id from facebook_event_checkin
where event_name = 'The Funky Grooves Tour')
limit 1 offset 2
);
```

<img src=  "https://github.com/Rubnserrano/apuntes/blob/main/imgs/sospechoso3.png?raw=true "/> 

Nos damos cuenta de que su id empieza por 48Z como dijo una de los testigos: **. The membership number on the bag started with "48Z ..., The man got into a car with a plate that included "H42W"** Vamos a mirar que coche tiene y que matricula a ver si coincide con lo que dijo la testigo
<img src=  "https://github.com/Rubnserrano/apuntes/blob/main/imgs/drivers_jeremy.png?raw=true "/> 
y efectivamente! Su matricula es 0H42W2.

```sql
INSERT INTO solution VALUES (1, 'Jeremy Bowers');
        
        SELECT value FROM solution;
```

Congrats, you found the murderer! But wait, there's more... If you think you're up for a challenge, try querying the interview transcript of the murderer to find the real villain behind this crime. If you feel especially confident in your SQL skills, try to complete this final step with no more than 2 queries. Use this same INSERT statement with your new suspect to check your answer.

El sujeto dijo esto:

I was hired by a woman with a lot of money. I don't know her name but I know she's around 5'5" (65") or 5'7" (67"). She has red hair and she drives a Tesla Model S. I know that she attended the SQL Symphony Concert 3 times in December 2017.

Para buscar esto en una sola query usamos una consulta con dos subqueries:

```sql
select * from person 
where id in (select person_id from facebook_event_checkin
where person_id in (select person.id from person
join drivers_license on person.license_id = drivers_license.id
where car_model = 'Model S' and height BETWEEN 65 and 67 and hair_color = 'red'));
```

y cuyo resultado es:

<img src=  "https://github.com/Rubnserrano/apuntes/blob/main/imgs/sospechoso_final.png?raw=true "/> 




Bibliografía: hackkerrank (ejercicios tipo leetcode) y sqlbolt.com (teoria + ejercicios)
