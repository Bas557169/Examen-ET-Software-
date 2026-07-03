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


# Fase 2 – Estrategia de Despliegue Avanzada (Blue-Green)
Objetivo
En esta fase se implementó una estrategia de despliegue Blue-Green en EKS, reemplazando la estrategia básica. El propósito fue garantizar continuidad del servicio mientras se introduce una nueva versión de la aplicación, validando su salud antes de redirigir el tráfico.

Archivos creados
1. .github/workflows/eks-bluegreen-deploy.yml
Propósito: Automatizar el despliegue Blue-Green con GitHub Actions.

Contenido:

Construcción y publicación de la nueva imagen (Green).

Despliegue de la versión Green.

Etapa de Validación de Salud.

Cambio de tráfico hacia Green mediante el Service.

Aporte: Garantiza que la nueva versión solo reciba tráfico si pasa la validación.

2. k8s/deployment-blue.yaml
Propósito: Representar la versión estable actual.

Contenido: Deployment con 2 réplicas, imagen etiquetada como blue.

Aporte: Mantiene la versión estable disponible mientras se prueba la nueva.

3. k8s/deployment-green.yaml
Propósito: Representar la nueva versión candidata.

Contenido: Deployment con 2 réplicas, imagen etiquetada como green.

Aporte: Permite desplegar la nueva versión en paralelo sin afectar la estable.

4. k8s/service-bluegreen.yaml
Propósito: Controlar el tráfico entre Blue y Green.

Contenido: Service tipo LoadBalancer con selector que apunta a la versión activa.

Aporte: Permite redirigir tráfico de Blue a Green de forma controlada.

Resultados de la Fase 2
Se logró tener dos versiones coexistiendo en el clúster (Blue y Green).

El pipeline asegura que la nueva versión se valide antes de recibir tráfico.

El Service permite cambiar de versión sin interrumpir el servicio.

Se garantiza continuidad y se minimizan riesgos al introducir actualizaciones.