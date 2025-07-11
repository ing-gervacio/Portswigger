En esta clase trabajamos con un entorno de XSS reflejado donde la mayoría de etiquetas HTML estándar están bloqueadas, pero algunas relacionadas con SVG aún están permitidas, incluyendo ciertos eventos compatibles con esa tecnología.

La vulnerabilidad está en el buscador, que refleja la entrada del usuario directamente en la respuesta. Aunque intentos típicos de inyección son bloqueados, realizamos un análisis sistemático usando Burp Intruder para descubrir qué etiquetas y atributos aún son aceptados por el sistema.

Tras probar múltiples combinaciones, identificamos que algunas etiquetas del entorno SVG, como svg y animatetransform, junto con eventos como onbegin, no son filtradas y permiten la ejecución de código al momento de cargarse la página.

Aprovechamos esto para construir un vector que, sin intervención del usuario, desencadena una función en el navegador al iniciarse la animación SVG, completando con éxito el laboratorio.

Este caso demuestra cómo incluso los entornos con filtros activos pueden ser vulnerables si se dejan rutas menos comunes abiertas, como el espacio SVG, y destaca la importancia de validar tanto etiquetas como atributos y eventos asociados

----

De igual forma, debemos de saber cuales son las etiquetas permitidas, podemos hacerlo con burpsite como los anteriores, desde el Intruder para buscar etiquetas validas

    /?search?=<etiqueta>

Ahora buscamos por los atributos validos:

    /?search?=<svg><animateTransform a=1>
    /?search?=<svg><animateTransform%20a=1>
    
Al finalizar el fuzzing, vemos que la etiqueta onbegin es correcta

    /?search?=<svg><animateTransform onbegin=1>

Ahora podemos aplicar el alert en la peticion

    /?search?=<svg><animateTransform onbegin=alert(0)>






