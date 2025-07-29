
En esta clase exploramos una técnica alternativa a XXE basada en la inyección de XInclude. A diferencia de los laboratorios anteriores, aquí no podemos definir una DTD personalizada, ya que el XML del servidor está parcialmente predefinido y no nos permite insertar declaraciones externas.

Para sortear esa limitación, aprovechamos la funcionalidad XInclude, que permite incluir archivos externos dentro de documentos XML válidos. En este caso, inyectamos una etiqueta ‘**xi:include**‘ con el espacio de nombres correspondiente y apuntamos al archivo /etc/passwd, usando ‘**parse=”text”**‘ para evitar errores de interpretación estructural.

El resultado es que el servidor incluye el contenido del archivo directamente dentro del XML final procesado, y este contenido se refleja en la respuesta. Es una técnica muy efectiva cuando se restringe la definición de entidades externas pero se permite el uso de XInclude.

-----
Buscar por XInclude Payload

Para este, caso debemos de incluir un fragmento de codigo:

```xml
<foo xmlns:xi="http://www.w3.org/2001/XInclude">
<xi:include parse="text" href="file:///etc/passwd"/></foo>
```

La inyeccion lo podemos poner en la siguiente data:

    productId=2&storeId=1

Agregamos el ataque:

    productId=<foo xmlns:xi="http://www.w3.org/2001/XInclude"><xi:include parse="text"     href="file:///etc/passwd"/></foo>&storeId=1

Y con esto, se resuelve el laboratorio


