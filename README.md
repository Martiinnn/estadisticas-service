# estadisticas-service

Microservicio de **estadísticas / dashboards** del casino (FastAPI, **solo lectura**).
Comparte la base de datos PostgreSQL y el `JWT_SECRET` con `casino-backend` (valida
el JWT del backend). Agrega KPIs sobre `transacciones`, `usuarios` y `apuestas`.

- Prefijo de rutas: `/api/estadisticas` · Docs: `/docs`

## Endpoints
| Método | Ruta | Descripción |
|---|---|---|
| GET | `/api/estadisticas/mias` | KPIs, desglose por tipo y línea de saldo del usuario |
| GET | `/api/estadisticas/globales` | Usuarios, GGR, top jugadores, métricas de apuestas |

## Ejecutar en local
```bash
python -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt
# variables: copia .env.example a .env y ajústalas
uvicorn app.main:app --reload --port 8006
```
Requiere una PostgreSQL accesible con las tablas compartidas que crea `casino-backend`.
Es de solo lectura: no crea ni modifica tablas.

## Despliegue en AWS EKS (Kubernetes) - EA3
Este servicio ha sido desplegado exitosamente en AWS EKS como parte de la Experiencia de Aprendizaje 3.

- **Rutas de salud**: Implementadas (`/health/liveness` y `/health/readiness`).
- **Docker**: Contenerizado mediante `Dockerfile` y alojado en **Amazon ECR**.
- **CI/CD**: Workflow de GitHub Actions configurado para construir y desplegar automáticamente en EKS.
- **Kubernetes**: Manifiestos de `Deployment`, `Service` y `HorizontalPodAutoscaler` aplicados.
- **Pruebas de Carga**: Validado mediante Locust con escalado automático (HPA) funcionando correctamente.
