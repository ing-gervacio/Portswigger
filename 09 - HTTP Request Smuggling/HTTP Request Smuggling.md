
# Mode: CL.TE

1. Cambiar el Protocolo de HTTP/2 a HTTP/1.1
2. Cambiar la petición de GET a POST (Si esto funciona, vamos por buen camino)
3. Primero debemos debemos de enviar un 0 al final de nuestra peticion con la cabecera "Transfer-Encoding: chunked" y enviar la peticion en la cola, para hacer un GET al recurso 404.

    ![](../Images%20Hacking/Pasted%20image%2020250730091909.png)

4. Ahora debemos de acceder al recurso /admin que no tenemos acceso

![](../Images%20Hacking/Pasted%20image%2020250730092104.png)

!!! Pero vemos que nos da un error, que solo se puede acceder desde usuarios locales

5. Para ello, metemos el Host para referenciar a usuarios locales:

![](../Images%20Hacking/Pasted%20image%2020250730092244.png)

!! Pero vemos que aun nos da error

6. Para bypassear, podemos poner mas cabeceras en la segunda peticion, como el CL y CT:

![](../Images%20Hacking/Pasted%20image%2020250730092334.png)

!!! Pero vemos que aun no es posible

7. Entonces cambiamos el "X-Ingore: X" y metemos un valor al cuerpo de la peticion:

![](../Images%20Hacking/Pasted%20image%2020250730092513.png)

!!! Logramos hacer el bypass, ya que podemos el panel de administrador

![](../Images%20Hacking/Pasted%20image%2020250730092555.png)


8. Entonces ya podemos borrar el usuario Carlos que es el objetivo de este ejercicio:

![](../Images%20Hacking/Pasted%20image%2020250730092717.png)


----

# Mode: TE.CL

Hacemos lo mismo que el CL.TE:


1. Cambiar el Protocolo de HTTP/2 a HTTP/1.1
2. Cambiar la petición de GET a POST (Si esto funciona, vamos por buen camino)
3. Primero debemos debemos de enviar un 0 al final de nuestra peticion con la cabecera "Transfer-Encoding: chunked" 

![](../Images%20Hacking/Pasted%20image%2020250730093110.png)

![](../Images%20Hacking/Pasted%20image%2020250730093150.png)

!! Pero vemos que no funciona, entonces no es CL.TE

4. Entonces como vemos que no es CL.TE, vamos a probar para el TE.CL, primeramente, debemos de desactivar la opcion "Update Content-Lenght"
5. Y mandamos como valor de CL a 4, para que solo tome los valores despues de la string "chunked" y el "XXX" y mandamos la segunda peticion con POST /404 HTTP/1.1 con su respectivo CL y CT y como valor en la data x=1 al final un "0" para marcar el final de la request y 2 saltos de linea.

    ![](../Images%20Hacking/Pasted%20image%2020250730093936.png)
    

6. Y antes de mandar la peticion, seleccionamos desde el valor "x=1" hacia arriba hasta el POST y contamos los caracteres  con el inspector (0x5e):
    ![](../Images%20Hacking/Pasted%20image%2020250730093805.png)
    
7. Y ese es el valor que tendrá donde tengamos el "XXX":

![](../Images%20Hacking/Pasted%20image%2020250730094027.png)

Y en la segunda respuesta de la peticion anterior, nos debe de dar el 404:

![](../Images%20Hacking/Pasted%20image%2020250730094157.png)

Entonces con esto, ya podemos decir que es un TE,CL

8. Entonces terminamos el ejercicio, accediendo al recurso /admin y cambiamos el valor de la peticion "5e" por el nuevo valor que nos da, que en este caso es 0x60, y lo reemplazamos:

    ![](../Images%20Hacking/Pasted%20image%2020250730094345.png)
    


9. Entonces la peticion final seria:

    ![](../Images%20Hacking/Pasted%20image%2020250730094440.png)


!!!Pero vemos que nos da un error, se debe a que no tenemos permisos, entonces agregamos el Host a localhost y volvemos a calcular el valor de toda la peticion final:

![](../Images%20Hacking/Pasted%20image%2020250730094622.png)

Y en la respuesta ya nos muestra el panel admin:

![](../Images%20Hacking/Pasted%20image%2020250730094656.png)

10. Para eliminar el usuario carlos, metemos el valor del recurso, y calculamos el valor de la ultima peticion y agregamos el valor del CL:

![](../Images%20Hacking/Pasted%20image%2020250730094853.png)

Tamaño de la peticion anterior:

![](../Images%20Hacking/Pasted%20image%2020250730094901.png)

![](../Images%20Hacking/Pasted%20image%2020250730094935.png)

Y finalmente, observamos que tenemos un 302 Found, y hemos finalizado el ejercicio.

![](../Images%20Hacking/Pasted%20image%2020250730095035.png)




