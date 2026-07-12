# 🛡️ Instalación de AdGuard Home en Docker
### 1. Nos meteremos en el directorio para instalar AdGuard desde docker
```text
/opt/docker/adguard
```
### 2. Y crearemos el siguiente docker-compose.yml
```dockerfile
services:
  adguard:
    image: adguard/adguardhome:latest
    container_name: adguard-home
    restart: unless-stopped
    ports:
      - "53:53/tcp"      # DNS TCP
      - "53:53/udp"      # DNS UDP
      - "8081:80/tcp"      # Web UI HTTP
      - "443:443/tcp"    # Web UI HTTPS / DoH
      - "443:443/udp"    # DoH UDP
      - "853:853/tcp"    # DoT
      - "784:784/udp"    # DoQ
      - "3000:3000/tcp"  # Setup inicial (remover después de configurar)
    volumes:
      - ./work:/opt/adguardhome/work
      - ./conf:/opt/adguardhome/conf
    environment:
      TZ: Europe/Madrid
    cap_add:
      - NET_ADMIN
      - NET_RAW
    healthcheck:
      test: ["CMD", "wget", "--quiet", "--tries=1", "--spider", "http://localhost:80"]
      interval: 30s
      timeout: 10s
      retries: 3
      start_period: 40s
```
### 3. Ejecutaremos el docker compose up -d
⚠️ Error de puerto 53 ocupado
Si nos sale el error:
```text
Error response from daemon: failed to set up container networking: driver failed programming external connectivity on endpoint adguard-home (id): failed to bind host port 0.0.0.0:53/tcp: address already in use
```
Ejecutaremos el siguiente comando para ver que ocupa el **puerto 53**:
```bash
sudo lsof -i :53
```
Si el ocupante es systemd-resolved
Si lo que nos está ocupando el puerto 53 es el systemd-resolve accederemos a su configuración con:
```bash
sudo nano /etc/systemd/resolved.conf
```
Y modificaremos:
```bash
#DNSStubListener=yes
```
a
```bash
DNSStubListener=no
```
Y reiniciaremos el servicio con:
```bash
sudo systemctl restart systemd-resolved
```
## 🚀 Configuración inicial
Una vez todo configurado accederemos a nuestra **IP:3000** para empezar la configuración
