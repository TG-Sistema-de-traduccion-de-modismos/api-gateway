# Kong API Gateway con Docker Compose

API Gateway implementado con Kong para gestionar el enrutamiento de solicitudes a servicios internos en máquinas virtuales de Google Cloud.

## 🚀 Descripción

Este proyecto configura un API Gateway usando Kong que expone dos endpoints principales:
- `/text` - Para procesamiento de texto
- `/audio` - Para procesamiento de audio

Ambos endpoints redirigen las solicitudes a sistemas internos desplegados en máquinas virtuales de Google Cloud.

## 📋 Prerrequisitos

- Docker
- Docker Compose
- Acceso de red a las máquinas virtuales de Google Cloud o al proveedor preferido

## 🔧 Configuración

### Ajustar direcciones IP

**IMPORTANTE:** Antes de ejecutar el proyecto, debes ajustar las direcciones IP en el archivo de configuración según tu infraestructura:

1. Abre el archivo de configuración de servicios de Kong
2. Modifica las URLs de los servicios `text` y `audio` con las IPs de tus máquinas virtuales:

```yaml
# Ejemplo de configuración
services:
  - name: text-service
    url: http://TU_IP_VM_TEXT:PUERTO
  
  - name: audio-service
    url: http://TU_IP_VM_AUDIO:PUERTO
```

## 🚀 Instalación y Ejecución

### Clonar el repositorio

```bash
git clone <URL_DEL_REPOSITORIO>
cd <NOMBRE_DEL_DIRECTORIO>
```

### Levantar los servicios

```bash
docker compose up --build -d
```

Este comando:
- Construye las imágenes necesarias
- Levanta todos los contenedores en modo detached (segundo plano)
- Configura Kong con las rutas `/text` y `/audio`

### Verificar el estado

```bash
docker compose ps
```

## 📡 Endpoints Disponibles

Una vez levantado el servicio, los endpoints estarán disponibles en:

```
https://modistra-api.duckdns.org/text   # Servicio de texto
https://modistra-api.duckdns.org/audio  # Servicio de audio
```

## 🛠️ Comandos Útiles

### Detener los servicios
```bash
docker compose down
```

### Ver logs
```bash
docker compose logs -f
```

### Reconstruir y reiniciar
```bash
docker compose up --build -d --force-recreate
```

### Ver logs de un servicio específico
```bash
docker compose logs -f kong
```

## 🔍 Verificación

Para verificar que Kong está funcionando correctamente:

```bash
# Verificar salud de Kong
curl -i http://localhost:8443/status

# Listar servicios configurados
curl -i http://localhost:8443/services

# Listar rutas configuradas
curl -i http://localhost:8443/routes
```

## 📝 Estructura del Proyecto

```
.
├── docker-compose.yml
├── kong.yml (o archivo de configuración)
├── README.md
└── [otros archivos de configuración]
```

## ⚙️ Puertos Utilizados

- `8000` - Puerto proxy de Kong (HTTP)
- `8443` - Puerto proxy de Kong (HTTPS)
- `8001` - Puerto Admin API de Kong
- `8444` - Puerto Admin API de Kong (HTTPS)

## 🔐 Consideraciones de Seguridad

- Asegúrate de que las demás máquinas tengan las reglas de firewall apropiadas
- En producción, deshabilitar el Admin API (puerto 8001) o restringir su acceso
- Configura HTTPS para los endpoints públicos
- Implementa autenticación en Kong según tus necesidades

## 🐛 Solución de Problemas

### Kong no inicia
```bash
docker compose logs kong
```

### No se puede conectar a las VMs
- Verifica las direcciones IP configuradas
- Comprueba las reglas de firewall
- Verifica que los demás servicios estén corriendo

### Errores de conectividad
```bash
# Probar conectividad desde el contenedor de Kong
docker compose exec kong curl http://IP_DE_TU_VM:PUERTO
```

## 📚 Recursos

- [Documentación oficial de Kong](https://docs.konghq.com/)
- [Docker Compose Documentation](https://docs.docker.com/compose/)


## 👥 Contribución

[Instrucciones de contribución si aplica]
