El sistema aplica un filtrado agresivo que codifica signos angulares, comillas simples y dobles, barras invertidas y las propias comillas invertidas, bloqueando así inyecciones clásicas. Sin embargo, no restringe las expresiones contenidas entre el símbolo de dólar y llaves, que son interpretadas como código ejecutable dentro de la plantilla.

Aprovechamos esta brecha introduciendo una expresión que se evalúa directamente al cargar la página, sin necesidad de romper el delimitador original. Al estar el contenido ya dentro de una cadena ejecutable, el navegador interpreta la expresión de inmediato y ejecuta la función deseada.

Este laboratorio demuestra cómo las cadenas de plantilla pueden ser un punto crítico de inyección si no se filtran expresiones embebidas, y destaca la importancia de neutralizar todos los elementos interpretables dentro de estructuras modernas de JavaScript.

-----

En este punto no podemos escapar de 

''
""
ni de las backticks

Por lo que tenemos que tener en consideracion lo siguiente desde la consola del navegador:

```javascript
var variable = "probando"

console.log("Mi variable es: "+variable);
console.log('Mi variable es: '+variable);

// Pero podemos hacer uso de los backticks:

console.log(`Mi variable es: ${variable}`);

// Entonces podemos insertar operaciones dentro del ${}

console.log(`Mi variable es: ${alert(0)}`);
```

Entonces en la entrada de datos, ponemos lo siguiente

${alert(0)}

Y el lab esta resuelto