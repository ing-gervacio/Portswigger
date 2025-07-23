Este laboratorio profundiza en la técnica de clickjacking permitiendo que el atacante modifique los datos del formulario a través de parámetros en la URL. En este caso, el objetivo es engañar al usuario para que pulse sobre el botón de actualizar correo electrónico, el cual ha sido rellenado automáticamente con una dirección maliciosa pasada por parámetro.

El ataque se basa en superponer un iframe con el formulario de cuenta sobre un botón señuelo que diga Click me. La URL del iframe incluye un parámetro email con el valor que se desea establecer. De este modo, el formulario se precompleta sin necesidad de interacción directa con el campo.

El usuario, al hacer clic en el señuelo, en realidad está enviando el formulario del iframe con el nuevo correo ya cargado. Este tipo de ataque demuestra que incluso sin vulnerabilidades en el backend, una interfaz mal protegida puede ser manipulada visualmente para obtener acciones críticas sin el conocimiento del usuario.

Es clave proteger estos formularios con cabeceras como X-Frame-Options o Content-Security-Policy y evitar la modificación de campos sensibles mediante parámetros URL.

-----

Debemos de engañar al usuario para que cambie su correo electronico

Primeramente, podemos ver que es posible cambiar el correo desde la URL, en el parametro email=markos, se observa que se rellena el campo del email, solo para que le demos clic y se actualice el correo

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
<iframe src="http://burpsute.com/?email=pwned@pwned.com"></iframe>
```