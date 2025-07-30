En esta clase trabajamos con una configuración CL.TE (Content-Length y Transfer-Encoding) donde el servidor frontal no acepta codificación chunked, pero el servidor trasero sí lo hace. El objetivo es confirmar si es posible inyectar una petición maliciosa al back-end mediante HTTP request smuggling y observar una diferencia en la respuesta como prueba de éxito.

Para ello, enviamos una primera petición que contiene una carga smuggleada que incluye una petición embebida hacia /404. Si el ataque funciona correctamente, al enviar una segunda petición a la raíz del sitio (/), el servidor responderá con un 404, lo que indica que la petición anterior fue procesada de forma inesperada por el back-end.

Esta técnica se basa en una diferencia en la interpretación de los encabezados ‘**Content-Length**‘ y ‘**Transfer-Encoding**‘ entre ambos servidores, lo que permite desincronizar las sesiones HTTP y comprobar la vulnerabilidad a través del comportamiento diferencial.

En esta clase continuamos con el mismo laboratorio anterior, en el que identificamos una vulnerabilidad de HTTP request smuggling basada en CL.TE (Content-Length + Transfer-Encoding). Mantenemos el mismo enfoque, pero seguimos profundizando en cómo se comporta el servidor al recibir múltiples peticiones dentro del mismo cuerpo HTTP.

El objetivo sigue siendo verificar que el ataque permite alterar la interpretación del contenido por parte del servidor de backend, forzando un error o manipulando las respuestas. Esta parte sirve como puente hacia la tercera y última sección del laboratorio, donde finalmente aprovecharemos este comportamiento para lograr el impacto deseado.

En esta clase finalizamos el laboratorio dedicado a la vulnerabilidad de HTTP request smuggling basada en CL.TE. Tras haber confirmado el comportamiento anómalo del servidor en las clases anteriores, aquí completamos la explotación para lograr el objetivo final del laboratorio.

Utilizamos el mismo punto vulnerable para smugglear una petición maliciosa que se interpretará de forma distinta entre el frontend y el backend, provocando que una solicitud aparentemente inocua desencadene una respuesta inesperada. Este ataque demuestra cómo un atacante podría manipular el flujo de peticiones HTTP para obtener acceso no autorizado o interferir con otros usuarios, cerrando así el ciclo completo de explotación de esta técnica.

----
#### CL = Content-Lenght
#### TE = Transfer Encoding

#### CL.TE = Quiere decir que el Front-End interpreta Content-Lenght y el Back-End interpreta Transfer-Encoding

![](Pasted%20image%2020250729172908.png)

![](Pasted%20image%2020250729173314.png)

![](Pasted%20image%2020250729173329.png)

![](Pasted%20image%2020250729173629.png)

![](Pasted%20image%2020250729173639.png)
![](Pasted%20image%2020250729173734.png)

----
# Ejercicio

1. Interceptamos la peticion princial y cambiamos el metodo de GET a POST y desde el Inspector->Request Attibutes cambiamos de protocolo HTTP/2 a HTTP/1.1
2. Quitamos todas las cabeceras que no necesitemos
3. Enviamos una peticion con la siguiente data:
    test=test

    En este caso se actualiza de manera automatica el CL de la peticion, si no queremos que haga eso, en el settings a lado de boton "SEND" deschekeamos "Update Content-Lenght" para tomar el control del valor del CL

4. Seleccionamos la casilla de "\n" que esta en la seccion de request para ver los salto de linea
5. Agregamos la siguiente cabecera a la peticion:

    Transfer-Encoding: chuncked

    Si mandamos esa peticion, hay un error, ya que provocamos una desincronizacion del back-end con el front-end

6. Agremos la siguiente data:

![](Pasted%20image%2020250729174846.png)

Ahora necesitamos provocar errores, y esta es una forma
![](Pasted%20image%2020250729175354.png)

Al igual que podemos hacer es hacer lo mismo que la imagen antepasada y cambiar el CL por un valor que no sea el correcto

Ahora, lo que necesitamos es realizar colgar una peticion despues de la primer peticion, jugando con la siguiente peticion:

![](Pasted%20image%2020250729180203.png)

Lo que hace es que despues que lea la primer peticion, enviamos la segunda peticion hacia un recurso que no exista y SIEMPRE debemos de agregar una cabecera cualquiera para que sea una peticion "COMPLETA"

Ahora cuando enviamos la peticion, nos devuelve el resultado de la primer parte de la peticion, y la respuesta de la otra peticion se queda en espera o en cola de respuesta, y la podemos atrapar o cachear despues de enviar otra solicitud, la cual veremos un error "404 not found"

Entonces podemos decir que una peticion que se quede colgada o en cola, y si un usuario hace una peticion, a ese usuario le puede llegar la peticion que mandamos, o viseversa

