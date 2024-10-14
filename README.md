# Proyecto NGINX con HAProxy

Este proyecto implementa una arquitectura de balanceo de carga utilizando **NGINX** como balanceador principal y **HAProxy** como balanceadores intermedios, que distribuyen el tráfico a varios servidores **NGINX**. La configuración se gestiona utilizando Docker Compose.

## Estructura del Proyecto

```bash
.
├── docker-compose.yml       # Archivo Docker Compose para levantar el entorno
├── haproxy.cfg              # Configuración de HAProxy
├── nginx/
│   ├── nginx1.conf          # Configuración de NGINX para el servidor 1
│   ├── nginx2.conf          # Configuración de NGINX para el servidor 2
│   ├── nginx3.conf          # Configuración de NGINX para el servidor 3
│   └── nginx4.conf          # Configuración de NGINX para el servidor 4
├── nginx-lb.conf            # Configuración del balanceador NGINX principal
└── www/
    ├── www1/
    │   └── index.html       # Página web para el servidor 1
    ├── www2/
    │   └── index.html       # Página web para el servidor 2
    ├── www3/
    │   └── index.html       # Página web para el servidor 3
    └── www4/
        └── index.html       # Página web para el servidor 4

Arquitectura de Balanceo de Carga
Este proyecto utiliza un esquema de balanceo en dos niveles:

Balanceador principal NGINX (nginx-lb): Este balanceador escucha en el puerto 80 y distribuye el tráfico entre dos instancias de HAProxy (haproxy1 y haproxy2).

Balanceadores intermedios HAProxy:

haproxy1: Escucha en el puerto 8081 y balancea las solicitudes entre los cuatro servidores NGINX.
haproxy2: Escucha en el puerto 8082 y realiza la misma función que haproxy1.
Ambos balanceadores también ofrecen estadísticas en los puertos 8404 y 8405 respectivamente.
Servidores NGINX: Hay cuatro servidores NGINX (nginx1, nginx2, nginx3, nginx4), cada uno sirviendo contenido estático distinto.

Servicios
El archivo docker-compose.yml define los siguientes servicios:

nginx-lb: Balanceador principal que distribuye las solicitudes entre haproxy1 y haproxy2. Está configurado para escuchar en el puerto 80.

HAProxy: Dos instancias de HAProxy (haproxy1 y haproxy2) que distribuyen el tráfico hacia los servidores NGINX.

haproxy1: Escucha en 8081 y en el puerto de estadísticas 8404.
haproxy2: Escucha en 8082 y en el puerto de estadísticas 8405.
NGINX: Cuatro instancias de servidores NGINX, cada una con su propia configuración y contenido estático.

nginx1: Configuración en nginx1.conf y contenido en www1/.
nginx2: Configuración en nginx2.conf y contenido en www2/.
nginx3: Configuración en nginx3.conf y contenido en www3/.
nginx4: Configuración en nginx4.conf y contenido en www4/.
Requisitos
Docker y Docker Compose instalados en tu máquina.
Instrucciones
Clona este repositorio:

bash

git clone <url_del_repositorio>
cd proyecto_nginx_haproxy
Levanta los contenedores usando Docker Compose:

bash
docker-compose up -d
El balanceador de carga principal NGINX estará disponible en http://localhost:80 y balanceará el tráfico entre las dos instancias de HAProxy.

Puedes acceder a las estadísticas de HAProxy en:

http://localhost:8404 para haproxy1
http://localhost:8405 para haproxy2
Personalización
Configurar NGINX: Puedes modificar los archivos de configuración dentro de la carpeta nginx/.
Contenido estático: Cambia las páginas web estáticas modificando los archivos index.html en las subcarpetas de www/.
HAProxy: Si necesitas cambiar las reglas de balanceo, edita el archivo haproxy.cfg.
