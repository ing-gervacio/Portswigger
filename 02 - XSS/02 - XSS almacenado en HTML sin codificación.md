
La vulnerabilidad se encuentra en la funcionalidad de comentarios de una entrada de blog. El sistema acepta entradas de texto —como nombre, email, sitio web y comentario— sin realizar ninguna codificación, validación o filtrado. Esto permite inyectar código que se guarda en el servidor y se ejecuta automáticamente cuando la página del blog vuelve a ser cargada por cualquier visitante.

En esta clase demostramos cómo insertar un fragmento de código que, al visualizarse en el blog, desencadena una acción en el navegador como mostrar una alerta, confirmando que el entorno es vulnerable.

Este laboratorio sienta las bases para comprender cómo el XSS almacenado puede ser utilizado para comprometer a múltiples usuarios, incluso sin interacción directa entre atacante y víctima.