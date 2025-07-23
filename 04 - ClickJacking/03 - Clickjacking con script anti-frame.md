Este laboratorio introduce una protección habitual contra ataques de clickjacking conocida como frame buster. Se trata de un script que impide que la página objetivo sea cargada dentro de un iframe, bloqueando así la superposición de contenidos engañosos. Sin embargo, el ataque sigue siendo posible si se neutraliza adecuadamente este mecanismo.

El truco consiste en usar el atributo sandbox dentro del iframe, permitiendo solo la ejecución de formularios mediante allow-forms. Esto impide que el script de ruptura del marco (normalmente basado en comprobar si ‘**window.top !== window.self**‘) se ejecute, ya que no tiene los permisos necesarios para acceder al contexto superior.

El atacante construye una página maliciosa que muestra un botón señuelo con el texto Click me y coloca justo debajo el iframe que contiene el formulario de actualización de email con el campo ya preestablecido a través de un parámetro en la URL.

Cuando la víctima hace clic en el señuelo, en realidad está enviando el formulario del iframe con el nuevo correo malicioso. A pesar del frame buster, el ataque funciona gracias al uso del sandbox correctamente configurado. Este laboratorio demuestra cómo ciertas defensas pueden ser insuficientes si no se acompañan de cabeceras como X-Frame-Options o políticas CSP estrictas

-----

Hacemos lo mismo que el script anterior, pero no es posible meter la URL en un iframe, ya que se interpreta el codigo javascript

buscamos "iframe sandbox que es"

Entonces, podemos usar "sandbox" para eludir esto gracias a los privilegios minimos, es decir, que no me interprete cierta información


```html
<style>
    iframe{
        width: 500px;
        height: 600px;
        opacity: 0.01; /*Para que no sea casi visible*/
    }
    div{
        position: absolute;
        top: 450px;
        left: 100px;
    }
</style>
<div>click me</div>
<iframe sandbox="allow-forms" src="http://burpsute.com/?email=pwned@pwned.com"></iframe>
```



