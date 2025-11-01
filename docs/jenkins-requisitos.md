# Requerimientos para Implementar Jenkins (Proyecto: El Poli Dev)

## 1. Objetivo y alcance
- **Objetivo:** Contar con un servidor de Integración Continua que, ante cada cambio en GitHub, ejecute **build + pruebas + empaquetado Docker** del backend de El Poli Dev.
- **Alcance inicial:** Pipeline en ambiente de integración (no prod): checkout → install → test → build imagen Docker → publicar artefacto.

## 2. Arquitectura propuesta
- **Jenkins Controller** (UI y orquestación).
- **Agente de build** basado en Docker (para ejecutar los jobs aislados).
- **SCM:** GitHub `tatiana189/el-poli-dev`.
- **Flujo:** GitHub (webhook) → Jenkins → Agente → Artefacto (imagen Docker guardada/archivada).

## 3. Requisitos de instalación
**Software**
- SO del servidor: Ubuntu 22.04 LTS (o equivalente).
- Jenkins LTS (idealmente en contenedor Docker).
- Docker Engine (para construir imágenes en los jobs).
- Git y acceso HTTPS al repo.

**Hardware recomendado (curso)**
- Controller: 2 vCPU, 4 GB RAM, 30 GB disco.
- Agente: 2 vCPU, 4 GB RAM, 20 GB disco.

**Red y puertos**
- Abrir **8080** (UI Jenkins) y **50000** (agentes inbound, si aplica).
- Salida a internet para dependencias (npm/Docker) y acceso a GitHub.

## 4. Despliegue de Jenkins
**Opción A – Docker (recomendada)**
- Ejecutar Jenkins en contenedor con volumen persistente `/var/jenkins_home` y, si el controller construye imágenes, montar `/var/run/docker.sock`.
**Opción B – Instalación nativa**
- Con JDK + servicio del sistema (solo si se exige).

## 5. Seguridad
- Autenticación: usuarios locales (curso).
- Autorización: **Matrix-based** con roles:
  - Admin CI (configuración y plugins)
  - Dev (disparar builds, ver logs/artefactos)
  - Viewer (solo lectura)
- **Credenciales:** tokens/contraseñas en *Jenkins Credentials*; inyección con `withCredentials`. Nunca subir secretos al repo.

## 6. Integración con GitHub
- **Webhook**: eventos `push`/`pull_request` → `https://<jenkins>/github-webhook/`.
- Pipeline as Code usando `Jenkinsfile` en el repo.
- Estrategia de ramas: `feature/*` (build y test), `dev` (build/test/empaquetado), `main` (build/test/empaquetado).

## 7. Plugins mínimos
- Pipeline, Git, GitHub, Credentials Binding
- Matrix Authorization Strategy
- Docker y Docker Pipeline (si se construyen imágenes dentro del job)

## 8. Pipeline (Jenkinsfile) – ejemplo inicial
```groovy
pipeline {
  agent any
  stages {
    stage('Checkout') { steps { checkout scm } }
    stage('Install & Test') {
      steps { dir('backend'){ sh 'npm ci'; sh 'npm test --if-present' } }
    }
    stage('Build Docker Image') {
      steps { sh 'docker build -t el-poli-dev-api:latest backend' }
    }
  }
}
