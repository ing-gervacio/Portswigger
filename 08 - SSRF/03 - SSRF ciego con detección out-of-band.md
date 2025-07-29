En esta clase continuamos con el estudio de ataques SSRF, pero en este caso nos enfrentamos a una variante ciega donde no obtenemos respuesta directa de la interacción. Aprovechamos una funcionalidad del sistema que realiza peticiones basadas en el valor del encabezado Referer, utilizado por un software de analítica interna.

Manipulamos este encabezado para incluir un dominio generado por Burp Collaborator, lo que permite detectar si el servidor realiza una solicitud saliente hacia dicho dominio. Aunque no veamos resultados en la respuesta de la aplicación, los logs de Burp Collaborator nos confirman que se ha producido una interacción, validando así la existencia de la vulnerabilidad. Esta técnica es fundamental para detectar SSRF en contextos donde no hay una retroalimentación directa.

----

Hay una cabecera Referer, en la cual se aplica el SSRF, hacemos un peticion al Collaborator de Burpsuite