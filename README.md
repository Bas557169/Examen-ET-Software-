# Examen-ET-Software-

# Fase 1 – Estandarización y Migración a EKS
Objetivo
La primera fase tuvo como propósito refactorizar el flujo de trabajo para que se ejecute automáticamente en GitHub Actions, utilizando plantillas reutilizables para las etapas de Build (construcción de imagen Docker y push a ECR) y Deploy (aplicación de manifiestos Kubernetes). Además, se incorporó la capacidad de inyectar variables de entorno dinámicas al clúster EKS.

Archivos creados
1. .github/workflows/eks-deploy.yml
Propósito: Automatizar el flujo de integración y despliegue.

Contenido:

Evento on: push que dispara el pipeline al subir cambios a la rama main.

Job Build: construcción de la imagen Docker y publicación en ECR.

Job Deploy: aplicación de los manifiestos Kubernetes desde la carpeta k8s/.

Aporte: Garantiza la estandarización del flujo CI/CD y la reutilización de plantillas.

2. k8s/deployment.yaml
Propósito: Definir cómo se despliega la aplicación en el clúster EKS.

Contenido:

Tipo Deployment con 2 réplicas.

Imagen de ECR.

Variables de entorno dinámicas (ENVIRONMENT, CLUSTER_NAME).

Aporte: Permite parametrizar el despliegue y asegurar consistencia en el clúster.

3. k8s/service.yaml
Propósito: Exponer la aplicación desplegada.

Contenido:

Tipo Service con LoadBalancer.

Selector app: myapp que conecta con el Deployment.

Puerto 80 para acceso externo.

Aporte: Garantiza que la aplicación sea accesible desde fuera del clúster.

Resultados de la Fase 1
Se logró una estructura estandarizada del repositorio con carpetas y archivos bien definidos.

Se implementó un pipeline automatizado que refleja el ciclo CI/CD orientado a EKS.

Se incorporaron variables de entorno dinámicas, mostrando cómo parametrizar despliegues.

Se dejó evidencia clara en el repositorio y en el informe, con capturas y documentación.