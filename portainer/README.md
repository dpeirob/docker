# 🐳 Instalación de Portainer en Docker
### 1.Lo primero será meternos en el directorio en el cual vamos a querer instalar nuestros dockers, en este caso será
```text
/opt/docker/portainer
```
### 2.Crearemos el fichero docker-compose.yml
Y pondremos los siguientes parámetros:
```text
services:
  portainer:
    container_name: portainer
    image: portainer/portainer-ce:lts
    restart: always
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
      - portainer_data:/data
    ports:
      - 9443:9443
volumes:
  portainer_data:
    name: portainer_data
networks:
  default:
    name: portainer_network
```
### 3. ⚙️ Opciones y para qué sirven
* `image: portainer/portainer-ce:lts` – Usa la versión LTS (Long Term Support) de la Community Edition, que recibe actualizaciones de seguridad prolongadas.
* `restart: always` – Garantiza que el contenedor se reinicie automáticamente después de reinicios del sistema o fallos.
* `/var/run/docker.sock:/var/run/docker.sock` – Montaje crítico que permite a Portainer comunicarse con el daemon de Docker del host.
* `portainer_data:/data` – Volumen persistente que almacena la configuración, usuarios y datos de la aplicación.
* `9443:9443` – Puerto HTTPS para acceso web seguro con certificado autofirmado.
* `8000:8000` – Puerto TCP para el servidor de túnel Edge (opcional, puedes eliminarlo si no lo usas).

Una vez creado ejecutaremos docker-compose up -d
