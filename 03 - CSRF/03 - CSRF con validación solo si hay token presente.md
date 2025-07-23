Mediante el análisis de las respuestas del servidor en Burp Suite, observamos que cuando el token es incorrecto, la solicitud es rechazada, pero si directamente eliminamos el parámetro, la validación desaparece y la operación se ejecuta correctamente.

Aprovechamos este fallo lógico para construir un ataque CSRF que omite por completo el campo csrf, lo cual permite modificar la dirección de correo electrónico del usuario víctima sin necesidad de conocer ni manipular un token válido.

Este caso demuestra un error de implementación típico: asumir que la ausencia de un token equivale a una solicitud segura, dejando expuesta la aplicación ante ataques triviales.

El servidor no valida el CSRF Token si lo quitamos directamente desde el metodo POST

