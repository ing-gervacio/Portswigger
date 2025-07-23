La funcionalidad vulnerable es el buscador del blog. Al introducir un valor en el cuadro de búsqueda, este se refleja dentro de un atributo HTML en la respuesta. Usando herramientas como Burp Suite, interceptamos la petición y comprobamos que nuestro valor aparece entre comillas dentro de ese atributo.

La estrategia consiste en cerrar el valor actual del atributo e introducir uno nuevo que contenga un manejador de eventos (por ejemplo, uno que se active al pasar el ratón). Al acceder a la URL modificada y mover el cursor sobre el área afectada, se ejecuta el código inyectado, demostrando que la inyección fue exitosa.

Este laboratorio destaca la importancia de validar correctamente no solo el contenido de las etiquetas, sino también lo que va dentro de atributos, donde también es posible ejecutar código si no se sanitiza correctamente

-----

Podemos inyectar codigo  javascript como un nuevo atributo sobre la etiquita principal

```html
<script>alert(0)</script>
"><script>alert(0)</script>
" 
" test="probando ///Esto seria un nuevo atributo sobre la etiqueta principal
" onmouseover="alert(0)   /// Esto me permite que cuando pase el cursor sobre el atributo, se ejecuta el alert
```

