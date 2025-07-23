
La aplicación vulnera al utilizar jQuery para localizar un enlace de navegación y modificar su destino utilizando directamente el valor de un parámetro en la URL. Al no realizarse ningún tipo de validación ni restricción sobre ese valor, es posible reemplazar el destino original con un esquema especial que ejecute código malicioso.

En este caso, sustituimos el valor del parámetro con una instrucción que, al hacer clic en el enlace, ejecuta una función para mostrar las cookies del navegador, demostrando así la explotación exitosa de la vulnerabilidad.

Este ejemplo evidencia cómo incluso operaciones aparentemente inofensivas, como cambiar el destino de un enlace, pueden volverse peligrosas si se basan en datos controlables por el usuario y se usan sin filtros adecuados

-----

Si esta dentro de la etiqueta href, puedes hacer lo siguiente

```html
javascript:alert(0)
```

