# Installation et Déploiement de vCluster Platform sur macOS

Ce guide explique comment installer et déployer **vCluster Platform** sur un MacBook Pro, ainsi que comment accéder à l'interface utilisateur (UI).

---

## **Prérequis**

### Outils requis :
1. **Homebrew** (gestionnaire de paquets pour macOS) :
   ```bash
   /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
   ```
2. **kubectl** (outil de gestion Kubernetes) :
   ```bash
   brew install kubectl
   ```
3. **helm** (outil de gestion des packages Kubernetes) :
   ```bash
   brew install helm
   ```
4. **vcluster CLI** (outil pour gérer les vClusters) :
   ```bash
   brew install loft-sh/tap/vcluster
   ```
5. Un cluster Kubernetes fonctionnel :
   - Installez **Docker Desktop** pour exécuter un cluster local (inclut Kubernetes) :
     ```bash
     brew install --cask docker
     ```
   - Activez Kubernetes dans les préférences de Docker Desktop.

---

## **Étapes d'installation**

### 1. **Vérifier le cluster Kubernetes**
Assurez-vous que Kubernetes est actif et fonctionnel :
```bash
kubectl config current-context
```
Si la sortie affiche `docker-desktop`, votre cluster est configuré.

Vérifiez que le cluster est opérationnel :
```bash
kubectl get nodes
```
Vous devriez voir une liste de nœuds avec l'état **Ready**.

### 2. **Déployer vCluster Platform**
Exécutez la commande suivante pour déployer la plateforme vCluster :
```bash
vcluster platform start
```

- Lors de l'exécution, vous serez invité à fournir une adresse e-mail pour l'utilisateur administrateur.
- Cette commande déploie la plateforme dans le namespace `vcluster-platform`.

### 3. **Vérifier le déploiement**

#### a) Vérifier l'état des pods :
Assurez-vous que tous les conteneurs sont en cours d'exécution :
```bash
kubectl get pods -n vcluster-platform
```
Vous devriez voir un pod avec l'état **Running**.

#### b) Vérifier les services exposés :
Listez les services disponibles :
```bash
kubectl get svc -n vcluster-platform
```
Si le service est de type **ClusterIP**, configurez un port-forward pour accéder à l'UI.

### 4. **Accéder à l'interface utilisateur (UI)**

#### a) Configurer un port-forward :
Si le service est de type **ClusterIP**, exécutez la commande suivante pour rediriger le port 80 du service vers le port 8080 de votre machine locale :
```bash
kubectl port-forward svc/vcluster-platform 8080:80 -n vcluster-platform
```

#### b) Accéder à l'UI dans le navigateur :
Ouvrez votre navigateur à l'adresse suivante :
```
http://localhost:8080
```

Vous serez automatiquement connecté et invité à créer un utilisateur administrateur si cela n'a pas été fait.

---

## **Commandes Utiles**

| Action                              | Commande                                                       |
|-------------------------------------|---------------------------------------------------------------|
| Vérifier les pods                  | `kubectl get pods -n vcluster-platform`                       |
| Vérifier les logs des pods         | `kubectl logs <pod-name> -n vcluster-platform`                |
| Vérifier les services exposés      | `kubectl get svc -n vcluster-platform`                        |
| Configurer un port-forward          | `kubectl port-forward svc/vcluster-platform 8080:80 -n vcluster-platform` |
| Arrêter vCluster Platform          | `vcluster platform stop`                                      |

---

## **Dépannage**

### Problèmes communs :

1. **Pods bloqués en "ContainerCreating" :**
   - Vérifiez les logs du pod pour identifier le problème :
     ```bash
     kubectl logs <pod-name> -n vcluster-platform
     ```
   - Assurez-vous que votre cluster Kubernetes dispose de suffisamment de ressources (CPU/mémoire).

2. **Accès à l'UI impossible :**
   - Assurez-vous que le port-forward est correctement configuré.
   - Confirmez que les pods sont en état **Running**.
  
<img width="761" alt="Screenshot 2025-01-03 at 20 45 44" src="https://github.com/user-attachments/assets/a20d3346-9b76-4808-8608-4f3a2c58a27f" />



  <img width="1423" alt="Screenshot 2025-01-03 at 12 00 02" src="https://github.com/user-attachments/assets/69a9a38d-a778-43bf-b699-b6a5cdd6d92d" />

     

3. **Service non exposé :**
   - Vérifiez les services disponibles dans le namespace `vcluster-platform`.

---

Avec ce guide, vous devriez être en mesure d'installer et d'utiliser vCluster Platform sur votre MacBook Pro avec succès. Si vous rencontrez des problèmes, n'hésitez pas à demander de l'aide  !

Hit the star  please!


