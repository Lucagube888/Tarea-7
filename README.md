# Configuración del Sistema DNS con Docker

Este proyecto configura un sistema DNS en Docker con un servidor Bind9 y un cliente Alpine que utilizan una red interna propia. Aquí se explican las configuraciones y procedimientos necesarios para cumplir con los requisitos especificados.

## Requisitos del Sistema

1. **Servidor DNS** (Bind9) y **cliente Alpine** en Docker.
2. Volumen separado para la configuración de Bind9.
3. Red interna dedicada para los contenedores.
4. IP fija para el servidor DNS.
5. Configuración de Forwarders en Bind9.
6. Creación de una zona DNS con registros:
    - **NS** (Name Server)
    - **A** (Address)
    - **CNAME** (Canonical Name)
    - **TXT** (Text)
    - **SOA** (Start of Authority)
7. Cliente con herramientas de red.

## Archivos del Proyecto

- `docker-compose.yml`: define los servicios de Docker.
- `named.conf.options`: archivo de configuración de opciones para Bind9.
- `named.conf`: configuración principal de Bind9 y definición de la zona DNS.
- `named.conf.local`: archivo para definir zonas DNS locales adicionales.
- `db.guarin.castelao.int`: archivo de zona con los registros de ejemplo para el dominio `guarin.castelao.int`.

## Explicación del `docker-compose.yml`

- container_name: Nombra el contenedor como server8 para identificar el servidor.
- image: Usa la imagen de Bind9.
- ports: Expone los puertos 53/udp y 53/tcp del DNS a través del puerto 56 del host.
- networks: Asigna una IP fija 192.168.5.1 en la red interna bind9_subnet.
- volumes: Monta la configuración y los datos de Bind9 desde las carpetas conf y zonas.

## Creación de Servicios (Contenedores)

- Ejecuta docker-compose up -d para iniciar los servicios en segundo plano.
- Usa docker-compose ps para verificar que los contenedores están activos y en la red interna.

## Modificación de Configuración, Arranque y Parada del Servicio Bind9
- Para modificar la configuración del servidor DNS, edita los archivos named.conf.options, named.conf, y named.conf.local en el directorio conf.

- Luego, reinicia el servidor con:

    docker-compose restart server

- Para detener el servicio Bind9, ejecuta:

    docker-compose down

## Configuración de la Zona DNS y Comprobación

- Forwarders: Configura los DNS forwarders en el archivo named.conf.options para enrutar solicitudes a DNS externos cuando el servidor no puede resolverlas.

- Zona: Define tu zona en el archivo named.conf.local.

- Registros: Dentro del archivo de zona (db.guarin.castelao.int).

- Comprobación: Para verificar que el DNS funciona, desde el cliente ejecuta:

    dig www.guarin.castelao.int
