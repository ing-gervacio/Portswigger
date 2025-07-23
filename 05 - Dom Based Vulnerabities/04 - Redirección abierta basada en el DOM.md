
En este laboratorio nos enfrentamos a una vulnerabilidad de tipo open redirection basada en el DOM. El comportamiento vulnerable se produce al hacer clic en un enlace que evalúa un parámetro url directamente desde el fragmento del location.href sin realizar ninguna validación.

El enlace ejecuta un pequeño script que extrae con una expresión regular el valor del parámetro url y redirige al usuario allí, lo cual puede ser aprovechado para enviar a la víctima a un dominio externo controlado por un atacante.

Para explotar el fallo, construimos una URL que apunta al sitio vulnerable incluyendo como valor del parámetro url la dirección del exploit server. Cuando la víctima accede, se realiza la redirección automática hacia el servidor del atacante.

-----

En la seccion del post, vemos que hay un elemento anchor, con algo inusual, por lo que podemos testearlo de esta forma:

    /url=(https:\/\/.+)/.exec(location)

Ahora desde la url de la pagina, agregamos lo siguiente:

    post?postId=5&url=https://setensa.com

Entonces pegalos la URL del exploit en url y listo

    post?postId=5&url=https://exploit-burpsuite.com

