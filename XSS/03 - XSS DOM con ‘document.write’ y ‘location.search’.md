
El laboratorio utiliza una función que escribe directamente en la página el valor obtenido desde la parte de búsqueda de la URL (lo que va después del símbolo de interrogación). Este dato no es validado ni codificado, y se inserta dinámicamente mediante una función que genera HTML de forma directa.

Inicialmente observamos que al hacer una búsqueda cualquiera, el valor se refleja dentro de un atributo de imagen. A partir de ahí, construimos un vector de ataque que rompe el atributo e introduce código que se ejecuta en el navegador de la víctima.

Este tipo de XSS es especialmente común en aplicaciones ricas en JavaScript y demuestra cómo la lógica del lado cliente puede ser tan peligrosa como una mala validación en el backend

----

https://lenguajejs.com/dom/introduccion/que-es

Podemos hacer pruebas para saber como se interpreta la data, en este caso, podemos verlo desde las tools del navegador

![](Pasted%20image%2020250703145003.png)

En este caso, en la linea sobreada en azul, se observa que se muestra el valor que insertamos en el campo "search", por lo que es posible controlar la entrada, hacemos lo siguiente:

```html
"><h1>Hola</h1>
"><script>alert(0)</script>
```


