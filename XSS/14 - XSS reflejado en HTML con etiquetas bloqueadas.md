La funcionalidad vulnerable es un buscador, donde el valor introducido se refleja directamente en el contenido HTML. Intentos clásicos de inyección son rechazados, por lo que utilizamos una estrategia sistemática con ayuda de Burp Suite.

Con Burp Intruder, realizamos una prueba automatizada enviando distintas etiquetas HTML y atributos, observando cuáles generan respuestas válidas. Detectamos que la etiqueta body y el atributo onresize no son filtrados. A partir de ahí, construimos un vector usando un contenedor invisible que al cargarse activa un evento de cambio de tamaño, el cual ejecuta directamente una función del navegador.

La prueba se completa al entregar el vector a una víctima a través de un servidor de explotación, validando así que la ejecución ocurre sin necesidad de clics o interacción adicional.

Este laboratorio muestra cómo es posible evadir sistemas de defensa si se analizan sus patrones de filtrado y se utilizan vectores alternativos cuidadosamente seleccionados.

-----

1. Vamos a tener varias etiquetas bloqueadas de HTML, porque debemos de buscar un via alterna de interpretar codigo

2. Pasamos la peticion por burp, y hacemos un intruder con todas las etiquetas del cheate sheet de XSS para saber cual es la que nos interpreta

3. Y los tags correctos son aquellos que me dan un codigo de estado 200 OK, por ejemplo body

        /?search=<body>

4. Ahora bien, podemos ver que eventos de body son validos

        /?search=<body evento=1>

5. Ahora con intruder hacemos lo mismo, copiamos todos los eventos y vemos cuales son correctos para usar con body

6. Una vez hecho el intruder sobre los atributos o eventos validos para body,  seria buscar algun evento que nos permita abusar de este, por ejemplo "onresize" 

        /?search=<body onrisize=print()>

7. Y esto se acontece, cuando hacemos zoom a la pantalla

8. Entonces ya teniendo el payload, se lo debemos de enviar a la victima desde el servidor que se tiene en burp, para ello haremos uso de la etiqueta iframe, para que cuando abra el exploit:

```javascript
<iframe src="https://bburpsuite.com/?search=<body onresize=print()>"></iframe>
```

9. Pero tenemos un problema, porque el usuario no sabe hacer zoom, para ello hacemos lo siguiente:

```javascript
<iframe src="https://bburpsuite.com/?search=<body onresize=print()>" onload=this.style.width='100px'></iframe>
```

10. En el cual, al realizar lo anterior, automaticamente se cambia de tamaño y provocamos el print()



