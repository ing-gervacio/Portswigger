La funcionalidad vulnerable se encuentra en el sistema de comentarios del blog. Para prevenir ataques, el sitio aplica una función de reemplazo que intenta codificar los signos angulares, pero lo hace incorrectamente: solo reemplaza la primera aparición en lugar de todas.

Aprovechamos este fallo insertando un par adicional de signos al principio del comentario. Estos primeros caracteres serán codificados y neutralizados, pero los siguientes no serán tocados, permitiendo que el navegador interprete y ejecute el contenido malicioso cuando se carga la página.

Este laboratorio demuestra cómo una protección mal implementada puede dar una falsa sensación de seguridad, y cómo es posible evadirla con simples técnicas de desbordamiento o saturación de filtros.

-----

https://hack4u.io/cursos/hacking-web/20125/

En la seccion de comentario, ponemos un payload para ver como se comporta la web

```
<script>alert(0)</script>
```

Y vemos que nos borra el </script>

y eso para porque esta usando una funcion, que nos permite cambiar los caracteres < y > por &lt; y &gt; pero con la funcion replace, pero replace solo nos quita el primer match que encuentre en la cadana, por lo que podemos inyectar lo siguiente

![](Pasted%20image%2020250709111741.png)

desde la consola del navegador podemos hacer pruebas:

```javascript

"cadena a probar".replace('<','&lt;').replace('>','&gt;');
"<><img src=0 onerror=alert(1)>".replace('<','&lt;').replace('>','&gt;');
```

<><script>alert(0)</script>

o poner mas variantes:

<><img src=0 onerror=alert(0)>