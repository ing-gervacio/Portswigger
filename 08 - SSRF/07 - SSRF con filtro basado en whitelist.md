
En esta clase trabajamos con un escenario en el que se ha implementado una defensa basada en lista blanca para mitigar ataques SSRF. A pesar de esta protección, conseguimos acceder a un endpoint interno aprovechando cómo el servidor interpreta las URLs con credenciales embebidas.

Comenzamos probando distintas variantes del parámetro ‘**stockApi**‘, observando cómo la aplicación extrae y valida el hostname. Tras verificar que se aceptan URLs con formato de usuario, utilizamos técnicas de doble codificación para manipular el valor del hostname y hacer que el servidor interprete la URL de forma diferente a como lo hace la validación.

Gracias a esto, logramos redirigir la petición al endpoint de administración alojado en ‘**localhost**‘, y ejecutamos la acción de eliminar al usuario objetivo. Esta clase muestra cómo se pueden eludir filtros aparentemente robustos manipulando los componentes de una URL.

----

1. El mismo procedimiento anterior, debemos de irnos al check stock, y vemos que se hace un peticion a traves del parametro stockApi
2. Vemos que no es posible, ya que nos manda error al intentar poner otro dato diferente al dominio que tiene por defecto
3. Ahora, a traves de una URL nos podemos autenticar, para ello buscamos "url authentication", para ello se muestra un ejemplo:

    http://username:password@example.com/

4. Entonces hacemos lo mismo para la peticion y vemos que lo deja pasar:

    stockApi=http://user:pass@stock.wellshop.net

5. Entonces, nos podemos aprovechar el autoscroll de la URL:

    stockApi=http://localhost:80#@stock.wellshop.net
    stockApi=http://localhost:80%2523@stock.wellshop.net -> urlencodeamos 2 veces el "#"

    Y podemos ver es posible la secccion de "Admin Panel"

6. Entonces para accedemos al Admin Panel, ponemos el recurso al final de URL:

    stockApi=http://localhost:80%2523@stock.wellshop.net/admin

7. Para borrar a Carlos hacemos lo siguiente:

    stockApi=http://localhost:80%2523@stock.wellshop.net/admin/delete?username=carlos



