# 📺 Instalación de Jellyfin 10.11.5 en Docker (Windows 10)
Esta documentación describe la instalación y configuración de Jellyfin 10.11.5 usando Docker Desktop en Windows 10.

## 📋 Requisitos
* Windows 10
* Docker Desktop instalado y en ejecución
* WSL2 habilitado
* Acceso al disco `C:` permitido en Docker Desktop

## 📁 Estructura de carpetas en Windows
Se recomienda crear las siguientes carpetas en el sistema host:
```text
C:\jellyfin\config
C:\jellyfin\cache
C:\Videos\Peliculas
```
### Descripción
* `config` → Configuración persistente de Jellyfin
* `cache` → Caché y datos temporales
* `Peliculas` → Archivos de vídeo
## 🐳 Creación del contenedor (docker run)
Ejecutar el siguiente comando desde PowerShell o CMD:
```powershell
docker run -d \
  --name jellyfin \
  --restart unless-stopped \
  -p 8096:8096 \
  -v C:/jellyfin/config:/config \
  -v C:/jellyfin/cache:/cache \
  -v C:/Videos/Peliculas:/media/peliculas \
  jellyfin/jellyfin:10.11.5
```

### ⚙️ Explicación de parámetros
* `-d`
Ejecuta el contenedor en segundo plano (modo detached).
* `--name jellyfin`
Asigna el nombre `jellyfin` al contenedor.
* `--restart unless-stopped`
Reinicia automáticamente el contenedor si Docker o Windows se reinician, excepto si fue detenido manualmente.
* `-p 8096:8096`

Mapeo de puertos:
* 8096 (host) → Acceso desde el navegador
* 8096 (contenedor) → Servicio Jellyfin

Acceso web:
```text
http://localhost:8096
```
`-v` (volúmenes)
```text
-v C:/jellyfin/config:/config
-v C:/jellyfin/cache:/cache
-v C:/Videos/Peliculas:/media/peliculas
```
| Host (Windows)      | Contenedor	| Uso |
|---------------------|------------|-----|
| `C:/jellyfin/config`|	`/config`|	Configuración|
|`C:/jellyfin/cache`  |	`/cache`|	Caché|
|`C:/Videos/Peliculas`|	`/media/peliculas`|	Películas|
> ⚠️ Dentro de Jellyfin **solo se usan rutas del contenedor**.

### Imagen
```text
jellyfin/jellyfin:10.11.5
```
Indica la imagen oficial de Jellyfin y la versión exacta utilizada.

## 🎬 Configuración de la biblioteca en Jellyfin
Durante la configuración inicial:
1. Añadir biblioteca
2. Tipo: Películas
3. Carpeta:
```text
/media/peliculas
```
4. Guardar y escanear

## 📂 Estructura recomendada de películas
```text
C:\Videos\Peliculas
├── Matrix (1999)
│   └── Matrix (1999).mkv
├── Inception (2010)
│   └── Inception (2010).mp4
└── Avatar (2009)
    └── Avatar (2009).mkv
```

## 🚫 Bloquear transcodificación
### 👣 Pasos
1. Entra a Jellyfin
2. Dashboard / Panel de control
3. Users / Usuarios
4. Selecciona el usuario
5. Ve a Playback / Reproducción
6. ❌ Desmarca:
* Allow video transcoding
* Allow audio transcoding
7. Guarda cambios

### 👉 Resultado
* ✅ Direct Play → funciona
*❌ Necesita transcode → NO reproduce
> 💡 Puedes hacerlo solo para ciertos usuarios (por ejemplo TVs).

### 📝 Notas finales
* Docker Desktop debe estar en ejecución para que Jellyfin funcione
* Los datos persisten gracias a los volúmenes montados
* El contenedor puede eliminarse sin perder configuración ni películas
