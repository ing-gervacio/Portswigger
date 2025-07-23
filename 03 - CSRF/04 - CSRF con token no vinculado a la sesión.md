En este laboratorio nos encontramos con una implementación de tokens CSRF que no están vinculados a la sesión del usuario. Esto significa que cualquier token válido puede ser utilizado por cualquier usuario, sin importar quién lo haya recibido originalmente.

Para explotar esta debilidad:

- Iniciamos sesión con un usuario y capturamos un token CSRF desde la funcionalidad de cambio de correo electrónico.
- Luego, iniciamos sesión con otro usuario y utilizamos ese token previamente obtenido para enviar una solicitud de cambio de correo.
- El servidor acepta el token aunque no pertenezca a esa sesión, lo que permite realizar el ataque CSRF.

Esta vulnerabilidad demuestra la importancia de vincular los tokens CSRF al contexto de la sesión del usuario que los recibió, de lo contrario, pueden ser reutilizados de forma maliciosa.

-----

debemos de loguearnos como dos usuarios al mismo tiempo, para ello debemos de hacer uso de 2 navegadores

Ahora los token son de un unico uso, para ello, mandamos la solicitud de un usuario por Burp, y copiamos el token de sesion del usuario y no lo usaremos, y damos DROP en burp para rechazar la peticion

En este caso, vamos a usar a Carlos como Token a reutilizar y a Peter como generador de la peticion maliciosa, lo que va a permitir

Nos aprovechamos de ese unico token para hacer el ataque y enviarlo a la victima



