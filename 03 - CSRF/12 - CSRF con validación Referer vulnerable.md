
Hacemos lo mismo que el Lab anterior

Tratamos de eliminar el valor del Referer

Añadir valores al inicio del valor del Referer

Por lo que podemos cambiar el nombre del exploit del server y ponerle un valor que acepte dentro del dominio principal de la aplicacion

Ahora, debemos de tomar en consideracion lo siguiente, buscar "unsafe-url referrer", y agregar al script lo siguiente en la seccion de Head:

"Referrer-Policy: unsafe-url"

Hacer el script como s4vitar




