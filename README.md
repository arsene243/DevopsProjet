# Déploiement d'une Application Web avec Kubernetes sur Minikube

Ce projet contient une application web simple avec un backend Django, un frontend React (placeholder), une base de données PostgreSQL et pgAdmin pour la gestion de la base de données. Ce guide explique comment déployer cette application sur un cluster Kubernetes local en utilisant Minikube.

## Prérequis

Avant de commencer, assurez-vous d'avoir les outils suivants installés sur votre machine :

*   [Docker](https://docs.docker.com/get-docker/)
*   [Minikube](https://minikube.sigs.k8s.io/docs/start/)
*   [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/)

## Déploiement

Suivez les étapes ci-dessous pour déployer l'application sur votre cluster Minikube.

### 1. Démarrer Minikube

Si Minikube n'est pas déjà en cours d'exécution, démarrez-le avec la commande suivante :

```bash
minikube start
```

### 2. Configurer l'environnement Docker

Pour que Minikube puisse utiliser les images Docker que vous allez construire localement, vous devez pointer votre client Docker vers le démon Docker de Minikube. Exécutez la commande suivante :

```bash
eval $(minikube docker-env)
```

### 3. Construire les images Docker

Construisez les images Docker pour le backend et le frontend en utilisant les commandes suivantes à la racine du projet :

```bash
docker build -t backend-service:latest ./backend
docker build -t frontend-service:latest ./frontend
```

### 4. Appliquer les manifestes Kubernetes

Une fois les images construites, vous pouvez déployer tous les composants de l'application en appliquant les manifestes Kubernetes qui se trouvent dans le répertoire `k8s/`. Utilisez la commande suivante :

```bash
kubectl apply -k k8s/
```

### 5. Vérifier le déploiement

Vous pouvez vérifier que tous les pods sont en cours d'exécution avec la commande suivante :

```bash
kubectl get pods
```

Vous devriez voir des pods pour le backend, le frontend, la base de données et pgAdmin.

## Accéder à l'application

Pour accéder aux différents services de l'application, vous pouvez utiliser la commande `minikube service` :

*   **Frontend:**
    ```bash
    minikube service frontend-service
    ```
*   **Backend:**
    ```bash
    minikube service backend-service
    ```
*   **pgAdmin:**
    ```bash
    minikube service pgadmin-service
    ```

Ces commandes ouvriront les services dans votre navigateur par défaut.

## Gestion des Secrets

Les informations sensibles, telles que les mots de passe de la base de données et de pgAdmin, sont gérées à l'aide de secrets Kubernetes. Les fichiers de déploiement sont configurés pour utiliser ces secrets, évitant ainsi de stocker des informations sensibles en clair dans les fichiers de configuration.

## Nettoyage

Pour supprimer tous les composants de l'application de votre cluster Minikube, exécutez la commande suivante :

```bash
kubectl delete -k k8s/
```
