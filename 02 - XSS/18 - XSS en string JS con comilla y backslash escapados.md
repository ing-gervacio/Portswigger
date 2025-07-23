
Debemos de probar como se comporta la entrada de la busqueda, insertando comillas, etc

Entonces nos esta escapando las comillas

Podemos cerrar la etiqueta <script> con </script>

Por lo que podemos agregar etiquetas despues de salir de contexto de script

```html
</script><h1>Test</h1>
```

Y observa que se interpreta el h1, ahora podemos colarle un script

```html
</script><script>alert(1)</script>
```

Y tendremos el laboratorio resuelto
