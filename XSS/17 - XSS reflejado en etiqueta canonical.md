En esta clase trabajamos con un XSS reflejado poco convencional, donde el punto vulnerable es una etiqueta utilizada para definir la URL canónica de la página. Aunque el sistema filtra los signos angulares para evitar aperturas de etiquetas nuevas, permite la inyección de atributos dentro de una etiqueta existente.

Aprovechamos este comportamiento para insertar atributos como accesskey y onclick, que nos permiten asociar una acción concreta —en este caso, la ejecución de código— a una combinación específica de teclas.

El exploit se construye de forma que, al presionar una combinación como Alt+Shift+X, el navegador dispare un evento que ejecuta la función deseada. Aunque no ocurre de forma automática al cargar la página, se considera válida ya que no requiere interacción directa con el contenido visible ni clics por parte del usuario.

Este laboratorio demuestra cómo atributos aparentemente inofensivos pueden ser utilizados como vectores de ejecución, y cómo el contexto de inyección —incluso en elementos como enlaces— puede tener implicaciones de seguridad si no se controla adecuadamente.

-----------

Abrimos el lab y vemos la etiqueta canonical

![](Pasted%20image%2020250710125636.png)

Entonces desde la web, podemos tratar de cerrar algunos etiquetas

https://burpsuite.wesecurity-academy.net/?'

Y es posible cerrar la comilla de "href"

![](Pasted%20image%2020250710130050.png)

Entonces podemos insertar atributos, para ellos podemos buscar accesskey html, e insertar el atributo accesskey

https://burpsuite.wesecurity-academy.net/?'accesskey='x'

Y podemos hacer uso de onclick para decirle que cuando presione la combinacion "ALT+X" se ejecute el alert

https://burpsuite.wesecurity-academy.net/?'accesskey='x'onclick='alert(0)





