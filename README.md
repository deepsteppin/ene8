# Dockerized n8n + PostgreSQL + WireGuard Setup

Este repositorio contiene una configuración lista para desplegar una instancia de **n8n**, **PostgreSQL** y **WireGuard** utilizando Docker Compose. Sigue los pasos detallados a continuación para configurar y desplegar este stack correctamente.

---

## **Requisitos previos**
1. **Docker** y **Docker Compose** instalados en tu máquina. 
   - Puedes seguir las instrucciones oficiales aquí:
     - [Instalar Docker](https://docs.docker.com/get-docker/)
     - [Instalar Docker Compose](https://docs.docker.com/compose/install/)

2. Asegúrate de tener configurado un entorno de red si `n8n_net` es una red externa. Para crear la red, ejecuta:
   ```bash
   docker network create n8n_net
   ```

## **Pasos para desplegar**

### **1. Clonar el repositorio**
Clona este repositorio y navega al directorio:

```bash
git clone https://github.com/deepsteppin/ene8.git
cd tu-repo
```

### **2. Configurar las credenciales**
1. Copia el archivo `.env.example` y renómbralo como `.env`:

```bash
cp .env.example .env
```

2. Edita el archivo `.env` para incluir tus credenciales y configuraciones reales:

```env
# PostgreSQL
POSTGRES_USER=mi_usuario
POSTGRES_PASSWORD=mi_contraseña_segura
POSTGRES_DB=mi_base_de_datos

# n8n
N8N_EMAIL=mi_email@example.com
N8N_PASSWORD=mi_contraseña_segura

# WireGuard
PEERS=2
```

**Nota:** El archivo `.env` se utiliza para definir las variables de entorno que serán cargadas por Docker Compose durante la inicialización de los contenedores.

### **3. Revisar el archivo `docker-compose.yml`**
Asegúrate de que el archivo `docker-compose.yml` está correctamente configurado. Las secciones críticas incluyen:
* Configuración de red: Verifica que `n8n_net` esté definido como externo si ya existe.
* Volúmenes: Asegúrate de que las rutas son accesibles y tienen los permisos necesarios.

Si estás desplegando en un entorno de producción, considera implementar medidas adicionales como Docker Secrets o volúmenes cifrados.

### **4. Levantar los servicios**
Para iniciar los servicios, ejecuta:

```bash
docker-compose up -d
```

### **5. Verificar el despliegue**
1. Verifica que los contenedores estén corriendo:

```bash
docker ps
```

2. Accede a n8n desde tu navegador:

```
http://localhost:5678
```

Si estás desplegando en un servidor remoto, reemplaza `localhost` por la IP o el dominio correspondiente.

## **Uso del archivo `.env`**
El archivo `.env` es fundamental para proporcionar credenciales y configuraciones a los servicios definidos en el archivo `docker-compose.yml`. Docker Compose lee automáticamente el archivo `.env` si se encuentra en el mismo directorio.

### **Cómo asegurarte de que el archivo `.env` sea detectado**
1. Coloca el archivo `.env` en el mismo directorio que `docker-compose.yml`.
2. No es necesario especificar manualmente el archivo `.env` en la mayoría de los casos. Sin embargo, si deseas indicar un archivo diferente, puedes usar la bandera `--env-file`:

```bash
docker-compose --env-file custom.env up -d
```

### **Protección del archivo `.env`**

* Establecer permisos restrictivos para el archivo:

```bash
chmod 600 .env
```

## **Comandos útiles**

### Ver logs
Consulta los logs de los servicios:

```bash
docker-compose logs -f
```

### Detener los servicios
Detén y elimina los contenedores:

```bash
docker-compose down
```

### Reiniciar un servicio específico

```bash
docker-compose restart <nombre_del_servicio>
```

## **Resolución de problemas**

### Contenedores no inician
1. Verifica que no haya un conflicto de puertos en el host.
2. Asegúrate de que las variables en el archivo `.env` sean válidas.

### n8n no puede conectarse a PostgreSQL
1. Confirma que las credenciales en `.env` coinciden con las configuraciones en `docker-compose.yml`.
2. Revisa los logs de PostgreSQL:

```bash
docker-compose logs postgres
```
