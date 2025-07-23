
El sistema aplica múltiples medidas de protección: codifica los signos angulares y las comillas dobles como entidades HTML, y además escapa tanto las comillas simples como las barras invertidas. Sin embargo, al observar cuidadosamente cómo se construye el atributo, descubrimos que aún podemos romper la estructura si inyectamos un valor cuidadosamente diseñado.

Utilizamos un valor de URL que contiene una secuencia manipulada para cerrar el contexto del atributo y ejecutar una función directamente. El carácter de escape que normalmente neutralizaría la comilla es absorbido por la estructura de la URL, permitiendo que el código se ejecute al hacer clic sobre el nombre del autor.

Este laboratorio enseña cómo incluso con múltiples capas de filtrado, pueden encontrarse formas de romper el contexto si se entiende cómo se evalúan los caracteres especiales en combinación con el navegador.

------
Practicamente, las comillas simples, los corchetes <>, comillas dobles y los backslash estan escapados

Entonces hay trucos que nos pueden permitir, representar comillas de igual forma, para eso abrimos un archivo html y ponemos lo siguiente:

```html
<p>It's a wonderful day</p>
```

Y con un servidor en python, podemos ver que se interpreta, pero buscamos representar la comilla simple de otra forma, para ello hacemos lo siguiente:

```html
<p>It&apos;s a wonderful day</p>
```

Y si vemos recargamos la pagina del servidor, se interpreta de manera correcta

Entonces podemos representar la comilla de la siguiente forma en el campo vulnerable

https://probando&apos;contenido

Y observa que nos interpreta de manera correcta la comilla, para ello hacemos lo siguiente para que nos lance el alert

https://probando&apos;+alert(0)+&apos

Y quedaria resuelto el ejercicio
