El fragmento dinámico se inserta dentro de una lista desplegable, por lo que cualquier valor enviado mediante ese parámetro se convierte en una nueva opción dentro del menú. Este comportamiento no incluye ninguna validación o codificación, lo que nos permite modificar la estructura del HTML de forma intencionada.

Para explotar la vulnerabilidad, rompemos la estructura del menú y añadimos un elemento adicional con un comportamiento malicioso, que se ejecuta automáticamente cuando el navegador intenta procesarlo.

Este laboratorio refuerza el concepto de que, cuando se generan elementos HTML a partir de datos controlables por el usuario, incluso dentro de componentes comunes como listas desplegables, puede abrirse una puerta directa a la ejecución de código si no se filtran correctamente las entradas

-----

Debemos de ver la funcionalidad de checker, pasamos la peticion por burp, y vemos que se envia data

Verificamos el valor de esa entrada, y se envia por post, pero en la pagina hay un script en js que puede ver que se puede concatenar un parametro en la url por post "& storeId=markos", y se te actualizará en la lista

Podemos hacer lo siguiente:

```javascript
productId=2&storedId=</options><h1>Hola</h1>
productId=2&storedId=</options></select><h1>Hola</h1>
```