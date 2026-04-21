# Resoluci-n-M-quina-Regla

Hago un resumen de los pasos seguidos para la explotaciónn de la máquina REGLA

Lo primero es poner en marcha la máquina vulnerable

<img width="1920" height="1140" alt="Captura de pantalla 2026-04-20 193215" src="https://github.com/user-attachments/assets/9d11810b-0008-459b-942f-36ec016a7527" />

# Enumración y reconocimiento

Una vez iniciada la máquina hacemos un escaneo de puertos para verificar los puertos abiertos en esta máquina así como tambien la versión de los servicios que se encuentren ejecutando en ella

<img width="1920" height="1140" alt="Captura de pantalla 2026-04-20 194202" src="https://github.com/user-attachments/assets/8242086b-537a-430a-855f-cbf815cde551" />

En vista de que los puertos que la máquina tenia abiertos procedi a la verificación en un navegador y me encontre con una página informativa hablandome un poco sobre los CVE en cuanto a SUDO en 2025

<img width="1920" height="1140" alt="Captura de pantalla 2026-04-20 203006" src="https://github.com/user-attachments/assets/57821751-32e7-469d-ad8b-85b02e7c2151" />

Hice un fuzzing utilizando la herramienta feroxbuster a ver si contenia algunos subdominios o carpetas que me fueran utiles, sin encontrar mas

Asi que procedi a verificar el codigo fuente de la máquina porque me imagine que en algun lugar de este debia haber algo que me funcionara y en efecto encontre un usuario encriptado

<img width="1920" height="1140" alt="Captura de pantalla 2026-04-20 203024" src="https://github.com/user-attachments/assets/52ea3c1f-9980-4e3b-92f8-2ba5b5a82193" />

Luego decodifique el username encontrado para dar con el nombre de este "alicia"

<img width="1920" height="1140" alt="Captura de pantalla 2026-04-20 204610" src="https://github.com/user-attachments/assets/fe551758-134a-4384-ab98-008ffa0e92f3" />

Ya teniendo este usuario es un avance bastante grande ya que como tenemos abierto el puerto #22 en esta maquina corriendo un servicio ssh, el paso a seguir es hacer un ataque de fuerza bruta en busqueda de la contraseña para este usuario

<img width="1920" height="1140" alt="Captura de pantalla 2026-04-20 205607" src="https://github.com/user-attachments/assets/8e19dd06-6086-4869-a651-8dee06fa0515" />

y Despues de un rato nos encontro la contraseña

<img width="1920" height="1140" alt="Captura de pantalla 2026-04-20 212803" src="https://github.com/user-attachments/assets/5a3270cf-405e-4f81-bce7-5d8d86a52238" />

# Explotación

Ya teniendo usuario y contraseña voy a intentar ingresar por medio de ssh, y obtengo acceso a la máquina

<img width="1920" height="1140" alt="Captura de pantalla 2026-04-20 223125" src="https://github.com/user-attachments/assets/1caa28d4-d969-45ac-921f-55239bc86f92" />

Liste algunos archivos en busqueda a ver si estaba la flag en algun otro lado pero no

<img width="1920" height="1140" alt="Captura de pantalla 2026-04-20 223201" src="https://github.com/user-attachments/assets/6228179c-64b9-4690-9083-4403b8f72415" />

# Escalada de privilegios 

Asi que el siguiente paso a seguir es buscar los binarios que tiene la máquina en busca de alguno que tenga el SUID activo

<img width="1920" height="1140" alt="Captura de pantalla 2026-04-20 224344" src="https://github.com/user-attachments/assets/3aae87a4-0cfb-40ac-bfb9-61f7fe45f421" />

Hice aguno intentos buscando un poco de información sobre esta vulnerabilidad que se encontro la cual es de SUDO; de igual manera como podemos observar en la imagen anterior pedi que me brindara la versión de sudo que estaba ejecutando la máquina y en efecto la 1misma era una versión desactualizada asi que procedi a buscar información de como podia explotarla

Lo primero que hice luego de esto fue buscar el nombre de los host en la configuración de red y encontre una entrada un poco irregular, el cual nos dice que este sistema esta configurado para reconocer un hostname llamado regla-server

<img width="1920" height="1140" alt="Captura de pantalla 2026-04-20 224737" src="https://github.com/user-attachments/assets/66d037a5-452d-40a8-b880-9c8bf873cd59" />

Luego de esto fui a identificar la regla y la misma me revelo el resultado (root) NOPASSWD: /bin/bash, el cual nos dice que el usuario llamado alicia podia obtener una shell como root pero solo si lo ejecuta desde el host llamado regla-server

<img width="1920" height="1144" alt="Captura de pantalla 2026-04-20 224809" src="https://github.com/user-attachments/assets/998aa54e-0771-4ccd-a7f4-d632bd6184b6" />

Luego de esto procedi a intentar escalar privilegios utilizando el siguiente comando que lo que hacia era que alicia ejecutara en el host llamado regla-server y abriera una shell como root y obtuve una respuesta positiva,, ya posicionandome como root en la máquina

<img width="1920" height="1140" alt="Captura de pantalla 2026-04-20 224932" src="https://github.com/user-attachments/assets/d8ce0ba2-beb3-4f35-a87d-51e7f7a91343" />

# Resultados

Ya dentro de la máquina procedi a buscar la flag.txt

<img width="1920" height="1140" alt="Captura de pantalla 2026-04-20 225015" src="https://github.com/user-attachments/assets/f9f6271f-3bb2-44f6-8c95-60274277979a" />

# *Powned*

<img width="1920" height="1140" alt="Captura de pantalla 2026-04-20 225028" src="https://github.com/user-attachments/assets/28309f3c-79b6-49f5-9789-4312a351d77d" />

