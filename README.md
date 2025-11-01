# el-poli-dev
Aplicación web El Poli Dev con Docker, GitHub Actions y PostgreSQL

## 📘 Documentación
- 🧾 [Requerimientos para implementar Jenkins](docs/jenkins-requisitos.md)

---

## 🐳 Implementación de Docker

### 🧾 Resultado esperado:

```nginx
NAME                STATUS    PORTS
elpolidev_backend   Up        0.0.0.0:3000->3000/tcp
elpolidev_db        Up        0.0.0.0:5432->5432/tcp


### 🧰 Comandos utilizados

Desde la carpeta raíz del proyecto (`~/el-poli-dev`):

```bash
cd infra
docker compose up --build
docker compose ps

Contenerizar la aplicación **El Poli Dev** para permitir la integración continua y el despliegue reproducible del backend y la base de datos PostgreSQL, garantizando que ambos servicios se ejecuten en entornos consistentes y comunicados entre sí.

### Estructura
Los archivos de configuración se encuentran en la carpeta `infra/`:

- `Dockerfile`: define la imagen del backend Node.js.  
- `docker-compose.yml`: orquesta los contenedores del backend (`elpolidev_backend`) y la base de datos (`elpolidev_db`).

### Servicios
| Servicio | Descripción | Puerto |
|-----------|--------------|--------|
| `elpolidev_backend` | Servidor Node.js que ejecuta la API de El Poli Dev | 3000 |
| `elpolidev_db` | Base de datos PostgreSQL 15 con configuración por variables de entorno | 5432 |

### Variables de entorno utilizadas
Estas variables están definidas en el archivo `docker-compose.yml`:

```yaml
POSTGRES_USER: postgres
POSTGRES_PASSWORD: postgres
POSTGRES_DB: elpolidev
