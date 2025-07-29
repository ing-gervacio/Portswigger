
En esta clase continuamos abordando ataques XXE avanzados. Esta vez, el laboratorio requiere que reutilicemos un DTD ya presente en el sistema de la víctima para redefinir una entidad y provocar una lectura de datos sensibles. Específicamente, aprovechamos el archivo ‘**docbookx.dtd**‘ que suele estar disponible en sistemas con entorno GNOME.

Definimos una entidad que referencia este DTD y redefinimos una entidad existente llamada ISOamso para incluir una carga maliciosa. Esta carga intenta leer el contenido del archivo passwd y generar un error utilizando esa información en una ruta inválida. Aunque el resultado no se muestra directamente, el error generado incluye el contenido leído, resolviendo así el laboratorio.

En esta clase, continuamos con el mismo laboratorio anterior, en el que reutilizamos un DTD local del sistema para realizar un ataque XXE. En esta segunda parte, terminamos de afinar la carga útil que redefine una entidad del archivo ‘**docbookx.dtd**‘ y genera un error controlado que nos permite exfiltrar el contenido del archivo /etc/passwd.

El objetivo es que, al provocar este error, el mensaje resultante contenga el contenido del archivo, lo cual permite resolver el laboratorio sin necesidad de respuestas visibles ni conexiones externas. Esta técnica es especialmente útil en entornos restringidos donde no es posible interactuar con servidores externos.

![](Pasted%20image%2020250723151319.png)

