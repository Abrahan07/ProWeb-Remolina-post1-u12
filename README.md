# Laboratorio Post-Contenido 1 — Unidad 12: Despliegue y CI/CD 
 
Contenedorización de aplicación Spring Boot y despliegue en Railway  
Programación Web — Ingeniería de Sistemas — UDES 2026

---

## Descripción

API REST de gestión de productos desarrollada con Spring Boot 3, contenedorizada con Docker mediante un Dockerfile multi-stage y desplegada en Railway con base de datos PostgreSQL.

**URL pública:** https://proweb-remolina-post1-u12-production.up.railway.app

---

## Tecnologías

- Java 21
- Spring Boot 4.0.6
- Spring Data JPA
- PostgreSQL 16
- Docker y Docker Compose
- Railway (plataforma de despliegue)

---

## Endpoints disponibles

| Método | Endpoint | Descripción |
|--------|----------|-------------|
| GET | `/api/productos` | Lista todos los productos |
| GET | `/api/productos/{id}` | Busca un producto por ID |
| POST | `/api/productos` | Crea un nuevo producto |
| DELETE | `/api/productos/{id}` | Elimina un producto |
| GET | `/actuator/health` | Estado de salud de la aplicación |

---

## Variables de entorno requeridas

| Variable | Descripción |
|----------|-------------|
| `SPRING_PROFILES_ACTIVE` | Perfil activo (`prod`) |
| `PGHOST` | Host del servidor PostgreSQL |
| `PGPORT` | Puerto del servidor PostgreSQL |
| `PGDATABASE` | Nombre de la base de datos |
| `PGUSER` | Usuario de la base de datos |
| `PGPASSWORD` | Contraseña de la base de datos |

---

## Construcción y ejecución local con Docker

### Prerrequisitos

- Docker Desktop instalado y en ejecución
- Git

### 1. Clonar el repositorio

```bash
git clone https://github.com/Abrahan07/ProWeb-Remolina-post1-u12.git
cd ProWeb-Remolina-post1-u12
```

### 2. Construir la imagen Docker manualmente

```bash
docker build -t remolina-post1-u12:local .
```

Verificar que la imagen fue creada correctamente:

```bash
docker images
```

La imagen debe aparecer con un tamaño inferior a 300 MB.

### 3. Levantar todos los servicios con Docker Compose

```bash
docker compose up -d --build
```

Verificar que ambos contenedores están corriendo:

```bash
docker compose ps
```

Ambos servicios `app` y `db` deben aparecer en estado `Up/healthy`.

### 4. Verificar el funcionamiento

```bash
# Health check
curl http://localhost:8080/actuator/health

# Listar productos
curl http://localhost:8080/api/productos

# Buscar producto por ID
curl http://localhost:8080/api/productos/1
```

### 5. Detener los servicios

```bash
docker compose down
```

Para eliminar también los volúmenes de datos:

```bash
docker compose down -v
```

---

## Despliegue en Railway

### Variables de entorno configuradas en Railway

En el panel de Railway, servicio de la aplicación → pestaña Variables, se configuraron las siguientes referencias al servicio PostgreSQL:

| Variable | Valor en Railway |
|----------|-----------------|
| `SPRING_PROFILES_ACTIVE` | `prod` |
| `PGHOST` | `${{Postgres.PGHOST}}` |
| `PGPORT` | `${{Postgres.PGPORT}}` |
| `PGDATABASE` | `${{Postgres.PGDATABASE}}` |
| `PGUSER` | `${{Postgres.PGUSER}}` |
| `PGPASSWORD` | `${{Postgres.PGPASSWORD}}` |

### Verificación del despliegue

```bash
# Health check en producción
curl https://proweb-remolina-post1-u12-production.up.railway.app/actuator/health

# Endpoints en producción
curl https://proweb-remolina-post1-u12-production.up.railway.app/api/productos
curl https://proweb-remolina-post1-u12-production.up.railway.app/api/productos/1
```

---

## Evidencias

### Docker local

**Captura 1 — `docker compose ps` mostrando servicios Up/healthy**  
![captura1](capturas/captura1.png)

**Captura 2 — Health check local `http://localhost:8080/actuator/health`**  
![captura2](capturas/captura2.png)

**Captura 3 — Endpoint `/api/productos` local con los 3 productos**  
![captura3](capturas/captura3.png)

### Panel de Railway

**Captura 4 — Panel de Railway con app y PostgreSQL en verde**  
![captura4](capturas/captura4.png)

**Captura 5 — Variables de entorno configuradas en Railway**  
![captura5](capturas/captura5.png)

### Endpoints en producción (Railway)

**Captura 6 — Health check en Railway `{"status":"UP"}`**  
![captura6](capturas/captura6.png)

**Captura 7 — Endpoint `/api/productos` en Railway**  
![captura7](capturas/captura7.png)

**Captura 8 — Endpoint `/api/productos/1` en Railway**  
![captura8](capturas/captura8.png)

---

## Estructura del proyecto

```
remolina-post1-u12/
├── src/
│   └── main/
│       ├── java/com/universidad/remolina_post1_u12/
│       │   ├── RemolinaPost1U12Application.java
│       │   ├── DataLoader.java
│       │   ├── controller/
│       │   │   └── ProductoController.java
│       │   ├── model/
│       │   │   └── Producto.java
│       │   ├── repository/
│       │   │   └── ProductoRepository.java
│       │   └── service/
│       │       └── ProductoService.java
│       └── resources/
│           ├── application.properties
│           └── application-prod.properties
├── capturas/
├── Dockerfile
├── .dockerignore
├── docker-compose.yml
└── README.md
```

---

