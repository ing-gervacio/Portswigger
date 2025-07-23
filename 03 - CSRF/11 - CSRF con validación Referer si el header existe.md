En este laboratorio se aprovecha una mala implementación del control de Referer como medida de protección contra ataques CSRF. Aunque la aplicación valida el encabezado Referer para asegurarse de que provenga del mismo dominio, acepta las peticiones incluso cuando este encabezado no está presente.

Mediante una metaetiqueta con name igual a referrer y content igual a no-referrer, se evita que el navegador incluya automáticamente el encabezado en la petición. Así, se logra que el servidor acepte la solicitud sin validar el origen, permitiendo cambiar el correo de la víctima sin romper ninguna política de seguridad aparente.

Este tipo de fallos demuestran cómo las medidas de seguridad basadas únicamente en encabezados pueden ser fácilmente burladas, especialmente cuando hay un fallback inseguro como en este caso.

---

Debemos de ver la cabecera Referer

Hacemos lo mismo de las clases pasadas, hacemos una PoC del formulario para enviarlo al server, pero habrá un error, ya que el Referer nos picha que no venimos de una pagina del dominio principal de la URL

Ahora eliminamos toda la cabecera Referer y vemos si funciona o no

Y Para quitar la cabecera hacemos lo siguiente al inicio del script malicioso:

```html
<html>
    <head>
        <meta name="referrer" content="no-referrer">
    </head>
<form>
    ........
</form>
<script>
    .......
</script>
</html>
```

