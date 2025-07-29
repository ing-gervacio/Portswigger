En esta clase exploramos un caso práctico de SSRF donde el servidor vulnerable interactúa con otro sistema interno que expone una interfaz de administración en la red privada. Aprovechamos la funcionalidad de verificación de stock para lanzar un escaneo controlado dentro del rango 192.168.0.X, variando la última parte de la IP.

Una vez identificamos qué IP responde en el puerto 8080 con una interfaz de administración accesible, manipulamos el parámetro vulnerable para interactuar con ella y ejecutar una petición que elimina al usuario carlos. Esta técnica demuestra cómo una SSRF aparentemente inocente puede convertirse en un vector para moverse lateralmente dentro de la infraestructura y comprometer otros servicios.

----

1. Debemos de buscar una maquina en la red 192.168.0.X con el puerto 8080 abierto
2. En vuln esta en el check stock
3. Entonces aplicamos lo siguiente: http://192.168.0.1:8080 iterando el valor del ultimo octeto de la URL
4. Con el intruder hacemos el descubrimiento
5. Y la IP valida es: http://192.168.0.45:8080
6. Entonces cargamos el recurso admin http://192.168.0.45:8080/admin
7. Y eliminamos a carlos: http://192.168.0.45:8080/admin/delete?username=carlos


