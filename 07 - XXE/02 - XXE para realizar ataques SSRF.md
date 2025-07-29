En esta clase continuamos explorando el potencial de las vulnerabilidades XXE, pero esta vez orientadas a realizar ataques de tipo SSRF (Server-Side Request Forgery). Aprovechamos el parser XML de la funcionalidad de comprobación de stock para que, en lugar de acceder a archivos locales, se conecte a un endpoint interno del servidor: la metadata de una instancia EC2 simulada en http://169.254.169.254/.

Mediante iteraciones en la URL de la entidad externa, accedemos progresivamente a los distintos directorios del endpoint hasta llegar a la ruta que expone las credenciales del rol IAM asignado a la máquina. Finalmente, logramos extraer la SecretAccessKey del servidor, evidenciando el riesgo real que supone una XXE con capacidad de SSRF mal controlada.

-----

Hacemos el mismo ejercicio anterior, para saber si hay un XXE, pero despues podemos cargar archivos locales del servidor a traves del SSRF, de la siguiente forma:

```xml
<!DOCTYPE foo [ <!ENTITY xxe SYSTEM "https://192.168.1.2/latest" > ]>
<productId>
&xxe;
</productId>
```

