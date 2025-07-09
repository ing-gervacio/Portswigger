La funcionalidad afectada es el sistema de seguimiento de búsquedas. Al introducir una consulta, el valor se refleja en una variable JavaScript como parte de una cadena. Esto permite al atacante cerrar esa cadena con comillas o caracteres especiales, insertar código adicional, y continuar la ejecución sin generar errores de sintaxis.

Utilizamos este enfoque para romper la cadena original y ejecutar una función maliciosa, demostrando así que la inyección es posible a pesar del filtrado parcial.

Este laboratorio introduce uno de los contextos más comunes y peligrosos en los que puede explotarse XSS: dentro del propio código del cliente, donde las medidas tradicionales de filtrado HTML no son suficientes para evitar el ataque.

------

El campo de busqueda es vulnerable a XSS, debemos de ver como se comporta a nivel de codigo y colar un alert

viendo el script de entrada, se puede ver lo siguiente si metener como cadena "test" (sin comillas)

```html
var searchTerm='test'
```

ahora podemos diseñar un payload para escapar la comilla y colar el alert:

```html

entrada = test'; alert(0); var comando='markos

y al sustituir en la query principal, se ve de la siguiente forma:

var searchTerm='test'; alert(0); var comando='markos'
```

y queda resuelto el ejercicio
