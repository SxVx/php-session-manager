# Conectarse a un servidor Redis con protocolo tsl

> Conexión Segura a un Servidor Redis con `redis-cli` y `stunnel`

Leer notas a pie de pagina


## Paso 1: Instalar stunnel

```bash
sudo apt-get update
sudo apt-get install stunnel4   # En Ubuntu/Debian
```

```bash
sudo yum install stunnel        # En CentOS/RHEL
```

```bash
brew install stunnel            # En macOS (usando Homebrew)
```


## Paso 2: Configurar stunnel para Redis

Crea el archivo de configuración para stunnel:

```bash
sudo vim /etc/stunnel/redis.conf
```

Agrega el siguiente contenido al archivo:

```bash
pid = /var/run/stunnel-redis.pid
client = yes

[redis-cli]
accept = 127.0.0.1:6379
connect = <IP_DEL_SERVIDOR_REDIS>:<PUERTO_DEL_SERVIDOR_REDIS>
```

Reemplaza **<IP_DEL_SERVIDOR_REDIS>** y **<PUERTO_DEL_SERVIDOR_REDIS>** con la dirección IP y el puerto de tu servidor Redis.

Guarda y cierra el archivo.

## Paso 3: Iniciar stunnel

Inicia stunnel usando el archivo de configuración:

```bash
sudo stunnel /etc/stunnel/redis.conf
```

## Paso 4: Conectar a Redis usando redis-cli

Usa redis-cli para conectarte a través del túnel seguro en el puerto local 6379:

```bash
redis-cli -h 127.0.0.1 -p 6379
```

La conexión está cifrada mediante TLS gracias a stunnel.

NOTA: Al conectarse desde aquí (en el contendedor) a redis con la configuración actual es lo mismo que conectarse a su localhost, por eso la dirección ip que muestra redis-cli es 127.0.0.1 pero las peticiones se hacen al servidor remoto

IMPORTANTE: Después de conectarte a redis con esta configuración si el servidor redis necesita autentificación no dejara leer ni escribir hasta autentificarse

```bash
$ redis-cli -h 127.0.0.1 -p 6379
127.0.0.1:6379> auth <password>
```


## NOTAS

1. Para hacer esto necesitamos conectarnos a nuestro contenedor docker que contiene redis

```bash
promt$ docker ps                

CONTAINER ID   IMAGE               COMMAND                  CREATED       STATUS       PORTS                                                       NAMES
a10f79a8a35a   redis:latest        "docker-entrypoint.s…"   3 hours ago   Up 3 hours   0.0.0.0:6379->6379/tcp, :::6379->6379/tcp                   redis
```

```bash
$ docker exec -it redis bash
```

2.- Verificamos sobre que sistema operativo esta nuestro contenedor
```bash
$ cat /etc/os-release
```

3.- Instalamos las dependencias que necesitamos y ya podemos seguir con normalidad lo descrito en los pasos para la conexión

```bash
$ apt-get update
$ apt-get install vim
```