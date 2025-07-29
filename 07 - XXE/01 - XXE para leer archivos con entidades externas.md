En esta clase resolvemos un laboratorio que permite la explotación de una vulnerabilidad XML External Entity (XXE) en una funcionalidad de comprobación de stock. Continuamos trabajando con peticiones XML personalizadas, donde aprovechamos la posibilidad de declarar entidades externas para acceder a archivos internos del sistema operativo.

En este caso, inyectamos una entidad que apunta al archivo /etc/passwd, el cual es devuelto en la respuesta cuando se intenta verificar un producto. Es una demostración clara de cómo una mala configuración del parser XML puede abrir la puerta a una fuga crítica de información en el servidor.

---

Para definir una entidad, hacemos lo siguiente

```xml
<!DOCTYPE foo [ <!ENTITY nombre "markos" > ]>
<productId>
&nombre;
</productId>
```

Ahora para cargar un documento, podemos hacer lo siguiente:

```xml
<!DOCTYPE foo [ <!ENTITY xxe SYSTEM "file:///etc/passwd" > ]>
<productId>
&xxe;
</productId>
```


