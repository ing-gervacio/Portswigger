En esta clase explotamos una vulnerabilidad de CSRF en combinación con la política SameSite=Lax. Aunque el navegador no envía cookies en peticiones POST cruzadas por defecto, si se logra refrescar la sesión del usuario a través de una redirección automática a /social-login, se emite una nueva cookie sin restricciones SameSite explícitas.

Sin embargo, esta redirección necesita interacción manual para evitar el bloqueo del popup. Por eso, el exploit incluye un mensaje para que el usuario haga clic, abriendo así la ventana y forzando el flujo OAuth. Una vez completado el proceso, tras una breve pausa, se lanza el ataque CSRF.

Este escenario demuestra cómo una mala implementación del control de sesiones unida a SameSite por defecto puede dejar aplicaciones vulnerables a ataques que dependen únicamente de ingeniería social y sincronización temporal

----


Primero debemos de saber que es SimeSite Lax

Ahora, debemos de ver todos los flujos completos a la hora de iniciar sesion, para ello, hacemos una PoC de CSRF, pero le añadimos lo siguiente:

```html
<script>
    window.open("https://burpsuite.com/social-login"); //Para obtener primero la cookie de sesion
    setTimeout(updateEmail, 5000); // esperamos 5 segundos y despues ejecutamos el script de arriba
    function updateEmail(){
        document.forms[0].submit();
    }
    
</script>



```

