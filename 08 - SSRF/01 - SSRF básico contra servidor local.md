En esta clase abordamos un ataque SSRF básico aprovechando una funcionalidad de consulta de stock que realiza peticiones internas. Aunque el panel de administración no es accesible directamente desde el navegador, al interceptar la petición que hace la funcionalidad de stock, modificamos el parámetro correspondiente para forzar una petición a ‘**localhost**‘.

Esto nos permite interactuar con la interfaz de administración interna, identificar la URL para eliminar al usuario carlos, y ejecutar dicha acción a través de la misma funcionalidad vulnerable. Con esto conseguimos resolver el laboratorio accediendo a recursos internos sin privilegios directos.

En esta clase continuamos con el mismo laboratorio anterior, donde explotamos una vulnerabilidad SSRF en la funcionalidad de stock. Ya demostramos que era posible acceder al panel de administración interno, y ahora profundizamos en cómo automatizar la interacción con rutas internas específicas.

El objetivo en esta parte es enviar una solicitud manipulada desde el parámetro vulnerable para ejecutar una acción concreta: eliminar al usuario carlos desde el panel de administración. Esto demuestra cómo un SSRF básico puede escalar rápidamente en impacto si se accede a funciones internas críticas del servidor.

------

1. Primeramente, la vuln esta en la peticion del check Stock
2. Cambiamos el recurso a http://localhost/admin
3. Para borrar el usuario carlos: http://localhost/admin/delete?username=carlos

