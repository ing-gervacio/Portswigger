
Podemos ver que esta mal configurada la cabecera de CSP, la cual manda una ruta con el parametro por GET y al añadirle el token=prueba, se te inyecta dentro del campo del CSP

Entonces podemos agregar una nueva politica dentro del CSP despues  del ";"

http://burpsuite.com/?search=<script>alert(1)</script>&token=; script-src-elem 'unsafe-inline'

Y con esto, estamos obligando que la CSP pueda ejecutar el codigo en javascript que no podiamos ejecutar gracias a las politicas creadas, pero con la nueva politica agregada fue posible

