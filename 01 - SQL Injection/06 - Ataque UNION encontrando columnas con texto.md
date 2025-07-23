La vulnerabilidad sigue estando en el filtro de categoría de productos, y como la respuesta de la aplicación muestra los resultados, podemos experimentar directamente.

El proceso es el siguiente:

- Primero se confirma cuántas columnas tiene la consulta, como ya se hizo en el laboratorio anterior.
- Luego se utiliza un valor aleatorio proporcionado por el propio laboratorio (como “abcdef”) y se prueba colocándolo en cada una de las posiciones ‘**NULL**‘ del payload, una a una.
- Si el valor aparece en la respuesta, significa que esa columna acepta datos tipo texto y puede usarse para mostrar información en futuras inyecciones.

Este paso es clave para que, en próximos ataques, podamos visualizar datos como nombres de usuarios, contraseñas, o cualquier otro campo sensible.

**Quédate con esto**: Saber qué columnas aceptan texto es imprescindible para extraer información visible mediante ataques UNION. Es un paso esencial en la preparación de una inyección efectiva.

-----

* Primero debemos de buscar el total de columnas existentes que nos muestra la Web
    + '  (para provocar un error en la consulta)
    + ' order by 3-- -
    + ' union select NULL,NULL,NULL-- -
    + ' union select NULL,NULL,'string'-- - (probar en todos los campos)
    + 