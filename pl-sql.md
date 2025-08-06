# PL/SQL

## Block Structure

PL/SQL es un lenguaje estructurado de bloques. 

Es decir, las unidades básicas (procedimientos, funciones y bloques anónimos) 
que componen un programa PL/SQL son bloques lógicos, que pueden contener cualquier número de subbloques anidados.

Normalmente, cada bloque lógico corresponde a un problema o subproblema a resolver. Por lo tanto, PL/SQL soporta el 
enfoque de divide y vencerás para la resolución de problemas llamado refinamiento paso a paso.

El orden de las partes es lógico. Primero viene la parte declarativa, en la que se pueden declarar los artículos. 
Una vez declarados, los elementos se pueden manipular en la parte ejecutable. Las excepciones planteadas durante la ejecución se pueden tratar en la parte de manejo de excepciones.

```
DECLARE
    -- declarations
BEGIN
    -- statements 
EXCEPTION
    -- handlers
END;
```

## Procedientos almacenados



Los procedimientos almacenados son solo una parte de las capacidades de PL/SQL, y la sintaxis de PL/SQL en su conjunto es más amplia para abordar una variedad de situaciones y requisitos de programación en la base de datos Oracle

La sintaxis de PL/SQL y los procedimientos almacenados puede variar debido a que PL/SQL es un lenguaje de programación procedural completo, mientras que los procedimientos almacenados 
son solo un tipo específico de construcción dentro de PL/SQL. PL/SQL proporciona un conjunto amplio de características y estructuras que van más allá de los procedimientos almacenados.

Un procedimiento almacenado es simplemente un tipo de objeto PL/SQL que encapsula una serie de sentencias SQL y lógica de programación.

Un procedimiento almacenado es un conjunto de instrucciones SQL y otras construcciones PL/SQL que se almacenan en un sistema de administración de bases de datos relacionales (RDBMS). 

Los procedimientos almacenados son programas compilados que pueden ejecutar sentencias de SQL. Un procedimiento almacenado típico contiene dos o más sentencias de SQL y proceso de manipulación o lógico en un programa. 

Los procedimientos almacenados se pueden llamar desde otras consultas o desde otros procedimientos almacenados. Un procedimiento puede tomar argumentos de entrada y mostrar valores como resultados. 

Los procedimientos almacenados también pueden recibir parámetros, lo que agrega cierta flexibilidad a la hora de escribir consultas SQL. 

```
PROCEDURE IngresarSolicitud(
    -- parametros del procedimiento --
) IS/AS

 -- declaracion de variables --
 -- declaracion de constantes --
 -- declaracion de cursores --

BEGIN
    -- logica
END

```

## Cursor

Un cursor es un conjunto de registros devuelto por una instrucción SQL.

Un cursor esta conformado por un conjunto de registros devueltos por una instruccion SQL del tipo select

Cursores implícitos. Este tipo de cursores se utiliza para operaciones SELECT INTO. Se usan cuando la consulta devuelve un único registro.

Cursores explícitos. Son los cursores que son declarados y controlados por el programador. Se utilizan cuando la consulta devuelve un conjunto 
de registros. Ocasionalmente también se utilizan en consultas que devuelven un único registro por razones de eficiencia. Son más rápidos.

El procesamiento de consultas de varias filas es algo así como el procesamiento de archivos. Por ejemplo, un programa COBOL 
abre un archivo, procesa registros y luego cierra el archivo. Del mismo modo, un programa PL/SQL abre un cursor, procesa las filas devueltas 
por una consulta y luego cierra el cursor. Así como un puntero de archivo marca la posición actual en un archivo abierto, 
un cursor marca la posición actual en un conjunto de resultados.

Un cursor explícito "apunta" a la fila actual en el conjunto de resultados. Esto permite que su programa procese las filas una a la vez.

Utilice las declaraciones OPEN, FETCH y CLOSE para controlar un cursor.

La instrucción OPEN ejecuta la consulta asociada con el cursor, identifica el conjunto de resultados y coloca el cursor antes de la primera fila.

La instrucción FETCH recupera la fila actual y hace avanzar el cursor a la siguiente fila.

Cuando se ha procesado la última fila, la instrucción CLOSE desactiva el cursor

PARA PROCESAR INSTRUCCIONES SELECT QUE DEVUELVAN MÁS DE UNA FILA, SON NECESARIOS CURSORES
EXPLICITOS COMBINADOS CON UN ESTRUCTURA DE BLOQUE.

## Cursor FOR Loops

En la mayoría de las situaciones que requieren un cursor explícito, puede simplificar la codificación utilizando un bucle FOR de cursor 
en lugar de las instrucciones OPEN, FETCH y CLOSE.

Un bucle FOR de cursor declara implícitamente su índice de bucle como un registro que representa una fila obtenida de la base de datos.

```
DECLARE
   CURSOR c1 IS
      SELECT ename, sal, hiredate, deptno FROM emp;
   ...
BEGIN
   FOR emp_rec IN c1 LOOP
      ...
      salary_total :=  salary_total + emp_rec.sal;
   END LOOP;
```

## Cursor Variables

Al igual que un cursor, una variable de cursor apunta a la fila actual en el conjunto de resultados de una consulta de varias filas.

## %TYPE

El atributo %TYPE proporciona el tipo de datos de una variable o columna de base de datos.

Esto es particularmente útil al declarar variables que contendrán valores de la base de datos.

Por ejemplo, asume que hay una columna llamada title  en la tabla books.

Para declarar una variable denominada my_title que tenga  el mismo tipo de dato que de la columna title de la tabla books, utilice la notación de puntos y el atributo %TYPE, 
de la siguiente manera: 

my_title books.title%TYPE;

## %ROWTYPE

Es un atributo que se utiliza para declarar una variable de tipo registro (record) con una estructura que coincide con una fila de una tabla o una vista en la base de datos.

ROWTYPE es lo mismo cuando instancias una clase. Con ROWTYPE se instancia una tabla lo que permite acceder a los valores de una columna en especifica de la fila capturada.

### Formas de uso

1. Asignar valores mediante SELECT INTO.

Cuando necesitas capturar valores desde la base de datos, puedes usar SELECT INTO para llenar el registro completo de una sola vez:

```
DECLARE
  -- Declarar un registro que tiene la misma estructura que una fila en la tabla employees
  v_employee_record employees%ROWTYPE;
BEGIN
  -- Capturar valores desde la tabla employees
  SELECT employee_id, first_name, last_name, salary
  INTO v_employee_record
  FROM employees
  WHERE employee_id = 1;

-- Capturar valores del registro.
v_employee_record.employee_id
v_employee_record.first_name
v_employee_record.last_name

  -- Ahora puedes trabajar con el registro lleno con los valores de la base de datos
  DBMS_OUTPUT.PUT_LINE('Employee Name: ' || v_employee_record.first_name || ' ' || v_employee_record.last_name);
END;
```

Es comunmente utilizada con select * into [nombre variable] from [nombre tabla] where [condicion] donde se captura en la variable la fila devuelta por la consulta, ya en adelante podras
acceder a los valores de la columna especificada, ejemplo: 

```
-- declaracion.
v_employee_record employees%ROWTYPE;  

-- Capturar valores del registro.
v_employee_record.employee_id
v_employee_record.first_name
v_employee_record.last_name
```

2. Asignar valores directamente.

%ROWTYPE es un atributo que se utiliza en la declaración de variables o registros para asociarlos automáticamente con la estructura de una fila en una tabla o vista. 
Este atributo es especialmente útil cuando trabajas con cursores o instrucciones SQL que devuelven conjuntos de filas.

Al usar %ROWTYPE, puedes declarar una variable o registro que tenga la misma estructura que una fila específica de una tabla o vista. Esto evita tener que declarar 
manualmente cada columna y su tipo de datos, lo que facilita la escritura de código y reduce la posibilidad de errores.

Puedes asignar valores directamente a cada campo del registro (ROWTYPE tipo de dato record = registro). No necesitas utilizar SELECT INTO para asignar valores. Aquí hay un ejemplo completo:





```
DECLARE
  -- Declarar un registro que tiene la misma estructura que una fila en la tabla employees
  v_employee_record employees%ROWTYPE;

BEGIN
  -- Asignar valores al registro (esto podría ser a través de un cursor, por ejemplo)
  v_employee_record.employee_id := 1;
  v_employee_record.first_name := 'John';
  v_employee_record.last_name := 'Doe';
  v_employee_record.salary := 50000;

  -- Puedes utilizar el registro en operaciones posteriores, como la inserción o actualización de datos
  -- Puedes realizar más operaciones aquí, como insertar este registro en otra tabla, etc.
  -- Por ejemplo:
  INSERT INTO employees (employee_id, first_name, last_name, salary)
  VALUES (v_employee_record.employee_id, v_employee_record.first_name, v_employee_record.last_name, v_employee_record.salary);
END;
/
```

En este ejemplo, v_employee_record tiene la misma estructura que una fila de la tabla employees debido al uso de %ROWTYPE. 
Esto facilita la manipulación de datos y evita la necesidad de especificar manualmente cada columna y su tipo de datos.


## Package

Le permite agrupar tipos, variables, cursores y subprogramas relacionados lógicamente en un paquete. 
Cada paquete es fácil de entender y las interfaces entre paquetes son simples, claras y bien definidas. Esto ayuda al desarrollo de aplicaciones.

Los paquetes suelen tener dos partes: una especificación y una cuerpo(Body). 

La especificación es la interfaz de sus aplicaciones; declara los tipos, constantes, variables, excepciones, cursores, procedimientos y subprogramas disponibles 
para su uso.

El cuerpo define cursores y subprogramas(procedimientos) y así implementa la especificación.

```
CREATE PACKAGE emp_actions AS  -- package specification
   PROCEDURE hire_employee (empno NUMBER, ename CHAR, ...);
   PROCEDURE fire_employee (emp_id NUMBER);
END emp_actions;

CREATE PACKAGE BODY emp_actions AS  -- package body
   PROCEDURE hire_employee (empno NUMBER, ename CHAR, ...) IS
   BEGIN
      INSERT INTO emp VALUES (empno, ename, ...);
   END hire_employee;
   PROCEDURE fire_employee (emp_id NUMBER) IS
   BEGIN
      DELETE FROM emp WHERE empno = emp_id;
   END fire_employee;
END emp_actions;
```

Sólo las declaraciones en la especificación del paquete son visibles y accesibles para las aplicaciones. 
Los detalles de implementación en el cuerpo del paquete están ocultos y son inaccesibles.

## Triggers

Un desencadenador de base de datos es un subprograma almacenado asociado con una tabla, vista o evento de base de datos. 
Por ejemplo, puede hacer que Oracle active un disparador automáticamente antes o después de que una instrucción INSERT, UPDATE o DELETE 
afecte a una tabla. Uno de los muchos usos de los activadores de bases de datos es auditar las modificaciones de datos. 

Por ejemplo, el siguiente disparador a nivel de tabla se activa cada vez que se actualizan los salarios en la tabla emp:

```
CREATE TRIGGER audit_sal
   AFTER UPDATE OF sal ON emp
   FOR EACH ROW
BEGIN
   INSERT INTO emp_audit VALUES ...
END;
```

# DATA DEFINITION LANGUAGE (DDL)

### CREATE DATABASE 
```
CREATE DATABASE databasename;
```

### CREATE TABLE
 ```
create table table_name (
    column1 datatype,
    column2 datatype,
    column3 datatype,
   ....
);
 ```

```
create table employees (
    id int not null,
    name varchar(255) not null,
    salary int,
    department varchar(255),
    position varchar(255)
);
```

### ALTER TABLE
Permite alterar la estructura de una tabla existente
 ```
alter table tablename
add (columnname1 datatype,
     columnname2 datatype,
     ...
     );
 ```

### DROP TABLE
Se utiliza para eliminar una base de datos SQL existente.
 ```
DROP TABLE table_name;
 ```

### SQL TRUNCATE TABLE
La declaración se utiliza para eliminar los datos dentro de una tabla, pero no la tabla en sí.
```
TRUNCATE TABLE table_name;
```

# DATA MANIPULATION LANGUAGE (DML)

## INSERT 
La instrucción INSERT INTO se utiliza para insertar nuevos registros en una tabla.
```
INSERT INTO table_name (column1, column2, column3, ...)
VALUES (value1, value2, value3, ...);
```

## UPDATE 
El comando ACTUALIZAR en SQL se utiliza para modificar los registros existentes en una tabla.
*La cláusula WHERE en la declaración UPDATE especifica qué registros modificar. Si omite la cláusula WHERE, ¡se actualizarán todos los registros de la tabla!
```
UPDATE table_name
SET column1 = value1, column2 = value2...., columnN = valueN
WHERE [condition];
```
`Set: Esta palabra clave se utiliza para establecer los valores de la columna.`

## DELETE 
La declaración DELETE se utiliza para eliminar registros existentes en una tabla.
```
DELETE FROM students WHERE student_id = '1001';
```
*Precaución: tenga mucho cuidado al utilizar la declaración DELETE. Si omite la cláusula WHERE, ¡se eliminarán todos los registros!

M students WHERE student_id = '1001';

## SELECT

## CLAUSE (WHERE, GROUP BY, HAVING, ORDER BY)

Syntax: 

```
SELECT column1, column2,...columnN 
FROM table_name
[WHERE]
[GROUP BY]
[HAVING]
[ORDER BY column(s) [ASC|DESC]]
```

### WHERE
La clausula where es usada para filtrar registros 
Operadores que pueden ser usados en la clausula WHERE

Operator	Description
    =           Equal
    >           Greater than
    <           Less than
    >=          Greater than or equal
    <=          Less than or equal
  <> or !=	    Not equal. In some databases, the != is used to compare values which are not equal.
 BETWEEN        Between some range
  LIKE          Search for a pattern
   IN           To specify multiple possible values for a column
  AND           Muestra el registros solo si todas las condiciones se cumplen.      
   OR           Muestra el registro si alguna de las condiciones es verdadera 
                Se utiliza para combinar multiples condiciones (para una o mas columnas)

### GROUP BY
Cuando usas la cláusula GROUP BY en SQL, estás agrupando filas basadas en los valores de una o más columnas. 
Por ejemplo, si agrupas por cod_poder, estás diciendo que quieres ver solo una fila por cada valor único de cod_poder.

Por eso, para evitar ambigüedades, en SQL se requiere que si estás utilizando GROUP BY, todas las columnas seleccionadas en tu consulta que 
no están siendo agregadas (mediante funciones como SUM, COUNT, AVG, etc.) deben estar presentes en la cláusula GROUP BY. 
Esto garantiza que haya una única fila representativa para cada combinación de valores agrupados.

Agrupa filas que tienen los mismos valores en filas resumen. Como ejemplo seria "encontrar el numero de clientes de cada pais"
Se usa a menudo con funciones count(), max(), min(), sum(), etc. Para agrupar el conjunto de resultados en un columna.

SELECT cod_cliente, SUM(cantidad)
FROM ventas
GROUP BY cod_cliente;

En SQL, cuando utilizas una función de agregación como SUM(), la columna sobre la que se aplica la función de agregación (en este caso, cantidad) 
no necesita estar incluida en la cláusula GROUP BY. Esto se debe a que la función de agregación opera sobre el conjunto de filas agrupadas, 
por lo que el resultado ya está agregado y no se necesita agrupar nuevamente por esa columna.

```
SELECT column_name(s)
FROM table_name
WHERE condition
GROUP BY column_name(s)
ORDER BY column_name(s);
```

### HAVING 
La clusula HAVING se agrego a SQL porque la clusula WHERE no puede ser usada con funciones sql.

Filtra los grupos de filas basados en una condición. La cláusula HAVING se aplica después de la agregación (es decir, después de que se hayan agrupado los datos), 
lo que significa que puede utilizar funciones de agregación en la condición.

Actua sobre las funciones de aggragate (funciones agregadas en sql como max(), mix(), etc)).

```
SELECT COUNT(CustomerID), Country
FROM Customers
GROUP BY Country
HAVING COUNT(CustomerID) > 5;
```

### ORDER BY
Puede ser usada para ordenar registros en una o mas columnas en order ASC o DESC 

```
SELECT column1, column2, ...
FROM table_name
ORDER BY column1, column2, ... ASC|DESC;
```

## CONDITIONAL OPERATORS (IN, <, >, =, LIKE, BETWEEN)
Operator	Description
    =           Equal
    >           Greater than
    <           Less than
    >=          Greater than or equal
    <=          Less than or equal
  <> or !=	    Not equal. In some databases, the != is used to compare values which are not equal.
 BETWEEN        Between some range
  LIKE          Search for a pattern
   IN           To specify multiple possible values for a column
   OR           Se utiliza para combinar multiples condiciones (para una o mas columnas)   

## OR
Se utiliza para especificar que al menos una de las condiciones debe ser verdadera para que se seleccione un registro
Se utiliza para combinar multiples condiciones (para una o mas columnas)

```
SELECT * FROM tabla
WHERE condicion1 OR condicion2;
```
*Esta consulta seleccionaría registros que cumplan con condicion1 o condicion2 (o ambas)*

## IN 
La cláusula IN se utiliza para especificar una lista de valores posibles para UNA columna particular.

Permite seleccionar registros donde el valor de esa columna coincida con cualquiera de los valores indicados.

```
SELECT * FROM tabla
WHERE columna IN (valor1, valor2, valor3);
```
*Si valor1 está presente en la columna, pero valor2 y valor3 no lo están, la consulta devolverá todos los registros 
donde la columna coincida con valor1 y los registros donde coincida con cualquier otro valor que esté en la lista de IN. 
Los valores ausentes simplemente se ignorarán.*

```
SELECT EmpId, FirstName, LastName, Salary
FROM Employee
WHERE EmpId IN (1, 3, 5, 6)
```
*La query anterior retornara los registros donde EmpId es 1 o 3 o 5 o 6.*

Permite especificar multiples valores para un columna en una clausula WHERE.
Es un atajo para multiples condiciones OR (solo si estas consultando para una misma columna).

También puedes usar IN con una subconsulta en la cláusula WHERE. Con una subconsulta puede devolver 
todos los registros de la consulta principal que están presentes en el resultado de la subconsulta.

```
SELECT column1, column2,..
FROM table
WHERE column IN (SELECT query)
```

## AGGREGATE FUNCTIONS
Las funciones agregadas de SQL son funciones incorporadas que se utilizan 
para realizar algunos cálculos sobre los datos y devolver un valor único. 
Por eso constituyen la base de las "consultas agregadas". Estas funciones 
operan en un conjunto de filas y devuelven un único resultado resumido.

- SUM
- COUNT
- AVG
- MIN
- MAX

### STRING FUNCTIONS
- CONCAT
- LENGTH
- UPPER
- LOWER
- REPLACE

### NUMERIC FUNCTIONS
- FLOOR
- ROUND
- Y MAS 

## CONDITIONAL 

### CASE

La expresion devuelve un valor cuando se cumple la primera condicion.
(como una declaracion if else). Una vez que que la condicion es verdadera, dejara de leer y devolvera el resultado.

```
case
    when condition1 then result1
    when condition2 then result2
    when conditionn then resultn
    else result
end
```

```
SELECT OrderID, Quantity,
    CASE
        WHEN Quantity > 30 THEN 'The quantity is greater than 30'
        WHEN Quantity = 30 THEN 'The quantity is 30'
        ELSE 'The quantity is under 30'
    END AS QuantityText
FROM OrderDetails;
```

```
SELECT
    CASE
        WHEN A+B<=C OR A+C<=B OR B+C<=A THEN 'Not A Triangle'
        WHEN A=B AND B=C THEN 'Equilateral'
        WHEN A=B OR B=C OR C=A THEN 'Isosceles'
        ELSE 'Scalene'
    END AS TRIANGLE
FROM TRIANGLES
```

## TRANSACTIONS

Una transaccion es un conjunto de operaciones (sentencias SQL) que van a ser tratadas como una unidad de trabajo (unit of work). 

Una transaccion se forma cuando hay una secuencia de una o mas sentencias SQL que se agrupan como una unidad logica de trabajo, y generalmente 
implica operaciones que modifican los datos.

Se consideran transacciones en SQL las operaciones que involucran cambios en la base de datos como INSERT, UPDATE, DELETE 
incluso operaciones de control de transacciones como BEGIN TRANSACTION, COMMIT y ROLLBACK. 

Una sentencia SELECT por si sola no se considera una transaccion en SQL. Una sentecia SELECT es una operacion de lectura que simplemente recupera datos de una base de datos sin modificarlos. 
Las transacciones en SQL generalmente involucran operaciones que modifican los datos como INSERT, UPDATE, DELETE.

**Sin embargo, es importante tener en cuenta que una sentencia SELECT también puede formar parte de una transacción más grande que incluya operaciones de modificación de datos. En este caso, la sentencia SELECT contribuiría a la transacción más grande, pero por sí sola no constituye una transacción completa.**

Estas transacciones deben cumpllir 4 propiedades fundamentales conocidas como ACID (atomicidad, coherencia, aislamiento, durabilidad)

Si una transaccion tiene exito, todas las modificaciones de los datos realizadas durante la transaccion se confirman y se 
convierten en una parte pemanente de la base de datos. Si una transaccion encuentra erroeres, se borran todas las modificaciones
de los datos.
Una transaccion es una serie de una o varias operaciones relacionadas con la BD
Se utilizan para garantizar  la integridad de los datos y manejar errores de la BD durante el procesamiento.


### BEGIN TRANSACTION
Comando para iniciar una transaccion
```
BEGIN TRANSACTION;
```

### COMMIT 
El comando COMMIT es el comando transaccional utilizado para guardar los cambios invocados por una transacción en la base de datos.
Cuando confirma la transacción, los cambios se guardan permanentemente en la base de datos.
```
COMMIT;
```

### ROLLBACK 
The ROLLBACK command is the transactional command used to undo transactions that have not already been saved to the database.
```
ROLLBACK;
```

### EJEMPLO
```
BEGIN TRANSACTION;

UPDATE Accounts SET Balance = Balance - 100 WHERE id = 1;
UPDATE Accounts SET Balance = Balance + 100 WHERE id = 2;

IF @@ERROR = 0
   COMMIT;
ELSE
   ROLLBACK;
```

### OTRO EJEMPLO
```
USE NorthWind
DECLARE @Error int
--Declaramos una variable que utilizaremos para almacenar un posible código de error

BEGIN TRAN
--Iniciamos la transacción
UPDATE Products SET UnitPrice=20 WHERE ProductName ='Chai'
--Ejecutamos la primera sentencia
SET @Error=@@ERROR
--Si ocurre un error almacenamos su código en @Error
--y saltamos al trozo de código que deshara la transacción. Si, eso de ahí es un
--GOTO, el demonio de los programadores, pero no pasa nada por usarlo
--cuando es necesario
IF (@Error<>0) GOTO TratarError

--Si la primera sentencia se ejecuta con éxito, pasamos a la segunda
UPDATE Products SET UnitPrice=20 WHERE ProductName='Chang'
SET @Error=@@ERROR
--Y si hay un error hacemos como antes
IF (@Error<>0) GOTO TratarError

--Si llegamos hasta aquí es que los dos UPDATE se han completado con
--éxito y podemos "guardar" la transacción en la base de datos
COMMIT TRAN

TratarError:
--Si ha ocurrido algún error llegamos hasta aquí
If @@Error<>0 THEN
	BEGIN
	PRINT 'Ha ecorrido un error. Abortamos la transacción'
	--Se lo comunicamos al usuario y deshacemos la transacción
	--todo volverá a estar como si nada hubiera ocurrido
	ROLLBACK TRAN
	END
```

## Optimización de consultas

1. Evitar el select * --> Se debe recuperar únicamente las columnas necesarias.
   
2. Usar indices --> Usa índices cuando tus consultas sean frecuentes y predecibles.
		--> Evita indexar todo indiscriminadamente, porque cada INSERT, UPDATE, DELETE también afecta los índices.	  
		--> Cada vez que haces un INSERT, UPDATE o DELETE, Oracle actualiza automáticamente los índices relacionados. (por eso las operaciones de escritura sean más lentas, Por eso, no se deben crear índices 		    innecesarios.)
   		--> Indexa solo columnas que realmente mejoran consultas (WHERE, JOIN, ORDER BY, JOIN en ON).
                    Si tu tabla es muy de lectura (consultas), más índices ayudan.
 		    Si es muy de escritura (transacciones frecuentes), menos es más.
   
3. Evitar WHERE CustomerPostcode LIKE ‘SE1% --> Los caracteres comodín pueden ralentizar los resultados de la consulta porque, la base de datos tiene que escanear toda la tabla (lo que se conoce como 						                escaneo de tabla) para encontrar resultados.

   
4. Evite la recuperación de datos redundantes o innecesarias --> Si bien la reducción de las consultas SELECT se centra en las columnas, también es importante limitar la cantidad de filas que se devuelven en una 									 consulta. Puede hacer esto utilizando LIMIT y restringiendo la devolución de datos a, por ejemplo, 100 o 200. Esta función evita que la consulta devuelva 								 miles de filas de datos cuando solo necesita utilizar unas pocas.

5. Utilice consultas EXIST() en lugar de COUNT() --> Al buscar un elemento específico en una tabla, es más eficiente utilizar una palabra clave EXIST() en lugar de una COUNT(). Esto se debe a que una consulta COUNT 							     cuenta cada instancia del elemento de búsqueda específico, lo que puede ser muy ineficiente, especialmente si la base de datos es grande.
                                                     Las consultas EXIST solo cuentan la primera aparición del elemento de búsqueda particular, lo que reduce los tiempos de búsqueda.
   
6. Evitar subqueries en clausulas WHERE o HAVING --> Pueden ralentizar el rendimiento de la consulta, ya que pueden devolver un gran número de filas, lo que dificulta su ejecución.
    						     Las cláusulas JOIN suelen ser una mejor opción.   				
