
Podemos enviar la peticion por POST, pero no por GET, entonces lo que podemos hacer es sobreescribir el metodo, y añadir lo siguiente en la peticion

    GET /change-email/?email=markos@markos.com&_method=POST

Haciendo esto, por detras, el servidor esta forzando a usar el metodo POST como metodo principal de la peticion, hacemos un CSRF PoC y se envia al cliente

