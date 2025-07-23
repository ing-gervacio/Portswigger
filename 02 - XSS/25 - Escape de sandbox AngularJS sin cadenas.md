El enfoque consiste en usar funciones nativas de JavaScript para construir cadenas de forma indirecta. Aprovechamos el método ‘**toString()**‘ y la propiedad ‘**constructor**‘ para acceder al prototipo de los objetos y redefinir cómo se comportan. En concreto, se sobrescribe el método ‘**charAt**‘ del prototipo de las cadenas, lo que permite eludir el sistema de seguridad interno de AngularJS.

Luego pasamos una expresión al filtro ‘**orderBy**‘, y generamos el código deseado utilizando ‘**fromCharCode**‘ con los valores numéricos correspondientes a los caracteres de la cadena ‘**x=alert(1)**‘. Como hemos alterado el comportamiento interno de las cadenas, AngularJS permite que esta expresión se ejecute donde normalmente estaría bloqueada.

Este laboratorio demuestra cómo es posible romper entornos supuestamente seguros mediante manipulación de bajo nivel, sin depender de comillas o funciones evaluadoras explícitas

-----


AngularJs hacia uso de sandbox, las cuales se pueden ver reprentadas en esta forma:

{{ user.name }} -> Esto se podia interpretar con HTML

Si queremos ver la version de AngularJs desde la consola del navegador, podemos hacer lo siguiente

angular.version.full

Podemos hacer uso del Cheat Sheet de Portswigger y filtrar por la version "1.4.4 without strings"

Que es un controlador de AngularJs

```javascript
$scope -> objeto especial que actua como puente entre la vista/controlador
$parse -> servicio que interpeta expresiones AngularJs user.name
```

Si agregamos en la URL un nuevo parametro con su valor, este sera visto desde el script de la pagina

http://burpsuite.com/?search=proband&prueba=1

Entonces nos podemos aprovechar del segundo parametro para efectuar una operacion

http://burpsuite.com/?search=proband&2+2=1   (Ponemos el + urlencodeado)

Entonces pegamos el siguiente codigo en la seccion de la operatoria anterior

```javascript
toString().constructor.prototype.CharAt=[].join;
[1]|orderBy:toString().constructor.fromCharCode(120,61,97,108,101,114,116,40,49,41)
``` 

De la siguiente forma:

http://burpsuite.com/?search=proband&toString().constructor.prototype.CharAt=[].join;[1]|orderBy:toString().constructor.fromCharCode(120,61,97,108,101,114,116,40,49,41)=1

Y quedaria resuelto el problema