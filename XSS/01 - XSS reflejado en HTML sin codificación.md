
En esta clase damos los primeros pasos en el mundo del Cross-Site Scripting (XSS), concretamente en su variante reflejada. Analizamos una funcionalidad de búsqueda donde el valor introducido por el usuario se refleja directamente en la respuesta HTML de la página, sin ningún tipo de codificación o validación.

Aprovechamos esta falta de protección para inyectar código que se ejecuta en el navegador de la víctima al interactuar con la funcionalidad vulnerable. En este caso, demostramos la ejecución de un fragmento de código que invoca una alerta en pantalla, lo cual confirma que la inyección es posible y activa.

Esta lección marca el inicio de una nueva sección dedicada a la explotación de XSS, explorando cómo los datos del usuario, si no son tratados correctamente, pueden convertirse en vectores de ataque dentro del navegador.