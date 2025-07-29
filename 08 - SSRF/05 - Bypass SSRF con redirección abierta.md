En esta clase resolvemos un caso de SSRF donde el stock **checker** sólo permite acceder a rutas internas de la propia aplicación, bloqueando directamente URLs externas. Sin embargo, identificamos una vulnerabilidad de redirección abierta en el parámetro path del endpoint ‘nextProduct’.

Aprovechamos esta redirección para hacer que el stock checker acceda de forma indirecta al panel de administración interno ubicado en http://192.168.0.12:8080/admin. Desde ahí, utilizamos la misma técnica para construir una URL que elimine al usuario carlos, completando así el laboratorio. Este enfoque muestra cómo una redirección abierta puede ser encadenada con un SSRF para saltarse restricciones de filtrado.

-----

1. Primero vamos al stock check
2. Si probamos, no podemos poner dominios en la consulta
3. En la parte inferior izquierda, hay una funcionalidad para de open redirect
4. Probamos haciendo lo siguiente:

https://burpsuite/product/nextProduct?currentProductId=3&path=/product/12

Ahora en path probamos hacer el redirect, y lo hace correctamente
https://burpsuite/product/nextProduct?currentProductId=3&path=https://google.es

5. Entonces nos aprovechamos del paso 2 y metemos la siguiente ruta:


    stockApi=/product/nextProduct?currentProductId=3&path=http://192.168.0.2:8080/admin

6. Para borrar el usuario:

    stockApi=/product/nextProduct?currentProductId=3&path=http://192.168.0.2:8080/admin/delete?username=carlos


