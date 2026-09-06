# SQL-CODER-UD5-ej2

### 1. ¿Cuántas filas devuelve cada consulta y por qué son distintas?

En este caso, ambas consultas devuelven 14 filas. `UNION` elimina únicamente las filas que sean completamente iguales en todas las columnas seleccionadas. Aunque algunos productos aparecen en ambas sucursales, sus valores de stock son diferentes.

Por ejemplo, el producto 103 aparece como:

- Norte: 103 | Monitor 4K 27" | Computación | 5
- Sur: 103 | Monitor 4K 27" | Computación | 3

Como el stock es diferente, las filas no son iguales y `UNION` no elimina ninguna. Lo mismo sucede con los productos 104 y 106.

Por lo tanto, `UNION` devuelve 14 filas y `UNION ALL` también devuelve 14 filas. En este caso, `UNION` no elimina ninguna fila porque no existen registros completamente duplicados.

### 2. ¿Por qué UNION ALL es más eficiente que UNION?

`UNION ALL` simplemente combina los resultados de ambas consultas y devuelve todas las filas. En cambio, `UNION` debe realizar una operación adicional para detectar y eliminar los registros duplicados.

Para realizar esta tarea, SQL Server puede utilizar operaciones como ordenamiento (`Sort`) o hashing (`Hash Match`), que requieren más recursos de memoria y procesamiento.

Por este motivo, `UNION ALL` suele ser más eficiente cuando no es necesario eliminar registros duplicados.

### 3. ¿En qué casos de negocio usarías cada uno?

Utilizaría `UNION` cuando necesito combinar información de diferentes fuentes y quiero evitar registros duplicados. Por ejemplo, podría utilizarlo para combinar una base de clientes proveniente de dos sistemas diferentes y obtener un listado único de clientes. Otro ejemplo sería combinar listados de empleados provenientes de diferentes bases de datos después de una migración, eliminando registros que sean exactamente iguales.

Utilizaría `UNION ALL` cuando necesito conservar todos los registros porque cada uno representa una operación o evento diferente. Por ejemplo, podría utilizarlo para combinar los registros de llamadas de dos períodos diferentes manteniendo cada llamada registrada. Otro caso sería combinar movimientos bancarios de diferentes períodos, donde cada depósito, retiro o transferencia debe conservarse aunque existan operaciones con valores similares.

### 4. ¿Qué pasa si las columnas de ambas consultas no coinciden en número o tipo?

Para utilizar `UNION` o `UNION ALL`, ambas consultas deben tener la misma cantidad de columnas y los tipos de datos de las columnas correspondientes deben ser compatibles.

Si las consultas tienen diferente cantidad de columnas, SQL Server genera un error indicando que las consultas combinadas mediante `UNION`, `INTERSECT` o `EXCEPT` deben tener la misma cantidad de expresiones en sus listas de selección.

Por ejemplo:

SELECT id_producto, nombre_producto
FROM inventario_sucursal_norte

UNION

SELECT id_producto
FROM inventario_sucursal_sur;

En este caso, la primera consulta devuelve dos columnas y la segunda solamente una, por lo que SQL Server genera un error.

También puede producirse un error cuando los tipos de datos no son compatibles. Por ejemplo, intentar combinar un campo `INT` con un `VARCHAR` puede generar un error de conversión si SQL Server no puede convertir los valores correctamente.
