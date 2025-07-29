# Ejercicio de WebApp Deployment con Kubernetes

Ejercicio práctico de Kubernetes que incluye Deployments, Services, ConfigMaps y Secrets.

## 🚀 Prerrequisitos

- Docker instalado
- Minikube instalado
- kubectl configurado

## 📋 Estructura del Proyecto

```
webapp-deployment/
├── deployment.yaml           # Deployment de la webapp
├── service.yaml             # Service para exponer la webapp
├── pod-with-config.yaml     # Pod con ConfigMap y Secret
└── README.md
```

## 🛠️ Pasos del Ejercicio

### 1. Iniciar Minikube
```bash
minikube start --driver=docker --memory=4096
```

### 2. Desplegar la Aplicación Web
```bash
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
kubectl get all
```

### 3. Acceder a la Aplicación
```bash
minikube service webapp-service --url
```

### 4. Escalar la Aplicación
```bash
kubectl scale deployment webapp-deployment --replicas=4
kubectl get pods
```

### 5. Actualizar la Aplicación (Rolling Update)
```bash
kubectl set image deployment/webapp-deployment webapp=nginxdemos/hello:plain-text
kubectl rollout status deployment/webapp-deployment
```

### 6. ConfigMap y Secret
```bash
# Crear ConfigMap y Secret
kubectl create configmap webapp-config --from-literal=ENV=production
kubectl create secret generic webapp-secret --from-literal=PASSWORD=supersecure

# Verificar recursos creados
kubectl describe configmap webapp-config
kubectl describe secret webapp-secret

# Aplicar pod que los usa
kubectl apply -f pod-with-config.yaml

# Verificar variables de entorno
kubectl exec webapp-with-config -- printenv | grep -E "ENV|PASSWORD"
```

## 🧹 Limpieza
```bash
kubectl delete -f deployment.yaml
kubectl delete -f service.yaml
kubectl delete -f pod-with-config.yaml
kubectl delete configmap webapp-config
kubectl delete secret webapp-secret
minikube stop
```

## 📚 Conceptos Aprendidos

- **Pod**: Unidad básica de despliegue
- **Deployment**: Gestión de réplicas y actualizaciones
- **Service**: Exposición de aplicaciones
- **ConfigMap**: Configuración no sensible
- **Secret**: Datos sensibles encriptados
- **Rolling Updates**: Actualizaciones sin tiempo de inactividad
- **Escalado Horizontal**: Ajuste dinámico de réplicas

## 🤔 Preguntas Finales y Respuestas

### ¿Cuál es la diferencia entre un Pod y un Deployment?

Un Pod es la unidad más pequeña en Kubernetes que representa uno o más contenedores, pero si falla se pierde para siempre. Un Deployment es más inteligente porque gestiona múltiples Pods a través de ReplicaSets, garantiza que siempre haya el número deseado de réplicas ejecutándose, tiene capacidades de auto-recuperación y permite hacer rolling updates sin tiempo de inactividad.

### ¿Por qué es útil usar un ConfigMap o un Secret?

Los ConfigMaps permiten separar la configuración del código siguiendo el principio de 12-factor apps, lo que significa que puedes usar la misma imagen con diferentes configuraciones por entorno sin reconstruirla. Los Secrets van más allá porque manejan datos sensibles con encriptación en reposo, control de acceso por RBAC y no aparecen en logs, permitiendo rotación segura de credenciales.

### ¿Qué ventajas ofrece Kubernetes frente a ejecutar contenedores manualmente con Docker?

Kubernetes automatiza la orquestación completa mientras que con Docker manual tienes que detectar fallas y recrear contenedores manualmente. Kubernetes ofrece self-healing automático, service discovery nativo, load balancing transparente, gestión declarativa donde describes el estado deseado y el sistema lo mantiene, rolling updates sin downtime y gestión inteligente de recursos con límites y requests.

### ¿Qué desafíos crees que enfrentaría esta app en producción?

En producción necesitarías implementar health checks robustos con liveness y readiness probes, configurar resource limits apropiados para evitar que un pod consuma todos los recursos del nodo, establecer network policies para seguridad, implementar monitoreo completo con métricas y logs centralizados, configurar horizontal pod autoscaling para manejar picos de tráfico, gestionar persistent volumes para datos importantes, y establecer pipelines de CI/CD con estrategias de deployment como blue-green o canary para minimizar riesgos.

## 🔗 Enlaces Útiles

- [Documentación oficial de Kubernetes](https://kubernetes.io/docs/)
- [Documentación de Minikube](https://minikube.sigs.k8s.io/docs/)
- [Hoja de referencia de kubectl](https://kubernetes.io/docs/reference/kubectl/cheatsheet/)

## 👨‍💻 Autor: Juan Pablo Gajardo

Ejercicio realizado como parte del aprendizaje de Kubernetes con WSL2 y Minikube.