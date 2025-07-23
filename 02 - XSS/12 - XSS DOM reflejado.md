La funcionalidad vulnerable está en el buscador del sitio. Al realizar una búsqueda, el término ingresado se refleja en un archivo JSON de resultados. Este contenido es posteriormente procesado por un script que usa una función que interpreta dinámicamente texto como si fuera código, abriendo la puerta a una inyección si no se escapan correctamente ciertos caracteres.

Aunque el sistema escapa comillas, no hace lo mismo con otros símbolos clave, como la barra invertida. Esto nos permite construir un valor malicioso que rompe la estructura del objeto y añade una instrucción personalizada, haciendo que se ejecute directamente en el navegador.

Este laboratorio muestra cómo una cadena aparentemente segura puede convertirse en un vector de ejecución de código si se mezcla con funciones peligrosas como la evaluación directa de datos, reforzando la importancia de evitar el uso de estas prácticas en el desarrollo web moderno.

---

https://hack4u.io/cursos/hacking-web/20124/

Primeramente probamos los tipicos script o payload para ver si se provoca un XSS, pero al final no es posible, ahora debemos de inspeccionar el codigo en la respuesta y hay un script de busqueda, lo analizamos con CURL

![](Pasted%20image%2020250709103357.png)

Vemos en las primeras lineas esta haciendo uso de la funcion EVAL, entonces buscamos en google qué es eso:

javascript eval function, el cual nos dice que es peligroso usar EVAL

El cual, con EVAL podemos hacer operatorias aritmeticas

```javascript
eval(2+1)
eval(2*1)
eval(2-1)
```

ahora, pasamos la solicitud por burp para saber qué recursos son los que manda llamar la aplicacion, y hay una petición hacia 

    /search-results?search=<input>

El cual, podemos ver el input que podemos insertar

    /search-results?search=probando

Y en la respuesta de la peticion, nos muestra la informacion en formato json, ahora probamos si podemos salir del contexto de json:

    /search-results?search=probando"
    /search-results?search=probando"hola

En esta esta peticion, es posible salir del contexto de json, porque escapamos las comillas y se interpreta la cadena "hola"

    /search-results?search=probando\"hola

Intentamos inyectar un alert

    /search-results?search=probando\"alert(0)

Pero hay un error en la sintaxis, entonces cerramos las llaves que hacen falta y adicional comentamos el resto de la sentencia

    /search-results?search=probando\"alert(0)}//

Vemos el alert no se interpreta de manera correcta, por lo que debemos de usar EVAL, insertando un valor aritmetico antes de la funcion alert

    /search-results?search=probando\"*alert(0)}//

Y si enviamos la cadena:

    probando\"*alert(0)}//

Se acontese el XSS, 







