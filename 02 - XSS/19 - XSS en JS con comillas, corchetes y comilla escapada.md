En esta clase analizamos un XSS reflejado que ocurre dentro de una cadena de texto en JavaScript. El valor reflejado del buscador se inserta en una variable delimitada por comillas simples. En este caso, las comillas simples están escapadas correctamente, y tanto los signos angulares como las comillas dobles están codificados como entidades HTML.

Sin embargo, la clave está en que las barras invertidas no se escapan, lo que nos permite introducir una de forma manual y romper el contexto de la cadena desde fuera.

Aprovechamos esta debilidad insertando una barra invertida seguida de una comilla escapada que finaliza la cadena, y después inyectamos una instrucción directa, separándola del código original con un operador y comentarios para evitar errores de sintaxis.

Este laboratorio muestra cómo es posible evadir filtros mixtos —HTML y JavaScript— si se detecta un punto débil en alguno de los mecanismos de escape, como en este caso con las barras invertidas no gestionadas correctamente.

-----

Podemos probar haciendo

testing'

Ahora probamos escapando la comilla, porque el script nos escapa las comillas

```html
testing\'
```

Ahora podemos insertar un atributo

```html
testing\'; var probando=markos
```

Vemos que no funciona, pero tenemos una forma alternativa de añadir texto a la cadena que se esta interpretando como variable, ponemos en la consola del navegador:

```html
var searchTerms = 'test'+alert(0)
```

esto seria valido, por lo que tambien se puede comentar el resto de la sentencia:

```html
var searchTerms = 'test'+alert(0);//
```

