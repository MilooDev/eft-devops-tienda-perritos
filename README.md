# 🐾 Plataforma Multicapa "Tienda Perritos" - Arquitectura Cloud & CI/CD

Este repositorio contiene el código fuente y la infraestructura como código (IaC) para la plataforma multicapa "Tienda Perritos". El proyecto implementa una arquitectura orientada a microservicios desplegada en AWS mediante metodologías DevOps, garantizando integración y entrega continua (CI/CD) a través de GitHub Actions.

## 🏗️ Arquitectura del Sistema

La solución está orquestada en **Amazon ECS** utilizando el motor de cómputo serverless **AWS Fargate**. Consta de tres capas desacopladas:
1. **Frontend (Capa de Presentación):** Servidor web Nginx (imagen basada en Alpine Linux) que expone la interfaz de usuario estática.
2. **Backend (Capa de Lógica de Negocio):** API RESTful desarrollada en Node.js, configurada con políticas de Service Auto Scaling (Target Tracking al 50% de CPU).
3. **Base de Datos (Capa de Persistencia):** Motor MySQL aislado perimetralmente.

## 📂 Estructura Detallada del Proyecto

El código está segmentado de forma modular para aislar responsabilidades y permitir la ejecución granular del pipeline CI/CD ante cambios específicos:

```text
eft-devops-tienda-perritos/
├── .github/
│   └── workflows/
│       ├── cicd-tienda-backend.yml      # Pipeline CI/CD independiente para la lógica de negocio
│       └── cicd-tienda-frontend.yml     # Pipeline CI/CD independiente para la presentación
├── backend/
│   ├── src/                             # Código fuente de la API
│   │   ├── controllers/                 # Controladores de rutas
│   │   ├── models/                      # Esquemas de base de datos
│   │   └── routes/                      # Definición de endpoints
│   ├── .dockerignore                    # Exclusión de módulos pesados locales
│   ├── Dockerfile                       # Construcción de imagen Node.js optimizada
│   ├── package.json                     # Gestor de dependencias
│   └── server.js                        # Punto de entrada de la aplicación
├── frontend/
│   ├── public/
│   │   ├── css/
│   │   ├── js/
│   │   │   └── app.js                   # Lógica de consumo del Backend (Fetch API)
│   │   └── index.html                   # Interfaz de usuario principal
│   ├── conf/
│   │   └── default.conf                 # Configuración estricta del enrutamiento Nginx
│   ├── .dockerignore
│   └── Dockerfile                       # Construcción multietapa (Builder -> Nginx Alpine)
├── db/
│   ├── init.sql                         # Script de inicialización de tablas (DDL/DML)
│   └── Dockerfile                       # Imagen de base de datos relacional
├── docker-compose.yml                   # Orquestación local y mapeo de redes
└── README.md                            # Documentación principal
