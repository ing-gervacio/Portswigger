El flujo del ataque es el siguiente:

- El navegador de la víctima primero realiza una petición GET a una ruta aparentemente inocua.
- Esa ruta incluye un parámetro que se inyecta directamente en el path de redirección usando JavaScript cliente-side.
- Con esa redirección controlada, conseguimos que el navegador envíe una petición GET autenticada al endpoint de cambio de email, el cual no exige tokens y permite método GET.

Esta técnica permite bypassear SameSite Strict, ya que la cookie de sesión se envía en la segunda petición, que es una navegación directa dentro del mismo dominio. Así conseguimos que el cambio de email se efectúe sin interacción directa del usuario.

Una clase muy potente para entender cómo pequeños detalles en la lógica del frontend pueden romper protecciones modernas como SameSite

-------

Podemos filtrar la peticion que le mandamos al cliente por burpsuite para ver su comportamiento, pero no nos agrega en la peticion la cookie de sesion

Vemos que en la respuesta de la peticion nos redirige a la /login porque no nos arrastra la cookie de sesion del usuario victima, y ademas vemos el atributo SameSite=Stric, lo cual nos bloquea para hacer ataques de CSRF

Por ello debemos de hacer todo el ataque sobre el mismo sitio o dominio de la pagina, para que no nos borre o se pierda la cookie de sesion

Ahora, podemos irnos a los articulos, en la seccion de comentarios, hay una peticion donde nos redirige al valor que le proporcionamos

Para ello, hacemos la redireccion desde el mismo dominiio, y URL encodiamos el "?" y "&"

