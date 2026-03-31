

# 🚀 **Application Web Java Spring Boot – DevOps CI/CD (Maven, Jenkins, SonarQube, ArgoCD, Helm, Kubernetes)**

Ce dépôt contient une application web Java basée sur **Spring Boot**, conçue pour être utilisée dans un pipeline CI/CD complet incluant :

* Maven
* Jenkins
* SonarQube
* Docker
* Helm
* Kubernetes
* ArgoCD

L’application suit l’architecture **MVC**, où un contrôleur renvoie une page contenant les attributs **title** et **message** à la vue.

---

# 📁 **Cloner ce dépôt**

```
git clone https://github.com/donaldte/Formation-Devops.git
cd /Formation-Devops/Jenkins-Zero-To-Hero/java-maven-sonar-argocd-helm-k8s/spring-boot-app
```

---

# 🛠️ Installer Maven sur Linux

## ✔️ Méthode simple (recommandée – Ubuntu/Debian)

```
sudo apt update
sudo apt install maven -y
```

Vérifier l’installation :

```
mvn -v
```

---

## ✔️ Méthode avancée (pour installer la dernière version officielle)

```
wget https://dlcdn.apache.org/maven/maven-3/3.9.6/binaries/apache-maven-3.9.6-bin.tar.gz
tar -xvzf apache-maven-3.9.6-bin.tar.gz
sudo mv apache-maven-3.9.6 /opt/maven
```

Ajouter Maven au PATH :

```
sudo nano /etc/profile.d/maven.sh
```

Ajouter :

```
export M2_HOME=/opt/maven
export MAVEN_HOME=/opt/maven
export PATH=${M2_HOME}/bin:${PATH}
```

Charger la configuration :

```
source /etc/profile.d/maven.sh
```

Vérifier :

```
mvn -v
```

---

# 📦 **Construire et exécuter l'application**

## 🔧 Générer les artefacts avec Maven

```
mvn clean package
```

Les artefacts seront générés dans le dossier `target/`.

Vous pouvez ensuite :

* Exécuter localement l’artefact
* L’utiliser dans Docker
* L’intégrer dans un pipeline CI/CD

---

# ▶️ **Exécution locale (nécessite Java 11 ou plus)**

```
java -jar target/spring-boot-web.jar
```

Accès à l’application :

👉 [http://localhost:8080](http://localhost:8080)

---

# 🐳 **Exécution avec Docker (méthode recommandée)**

## 1. Construire l'image Docker

```
docker build -t ultimate-cicd-pipeline:v1 .
```

## 2. Lancer le conteneur

```
docker run -d -p 8010:8080 -t ultimate-cicd-pipeline:v1
```

Accès :

👉 http://<ip-address>:8010

---

# 🧭 **Configuration d'un serveur SonarQube local**

## ✔️ Prérequis système

* Java 17+ (Oracle JDK, OpenJDK ou AdoptOpenJDK)
* Minimum 2 Go RAM
* 2 CPU


# 🔗 **Intégration des outils DevOps**

Ce projet est conçu pour fonctionner avec :

### ✔️ Jenkins (CI)

* Build Maven
* Analyse SonarQube
* Construction d’image Docker
* Push Docker Hub ou ECR

### ✔️ SonarQube (Code Quality)

* Analyse du code Java
* Quality Gate
* Intégration Jenkins via webhook

### ✔️ Helm (Packaging)

* Déploiement de l’application sous forme de Chart Helm

### ✔️ Kubernetes (Orchestration)

* Déploiement via kubectl ou Helm
* Exposition via Service NodePort ou Ingress

### ✔️ ArgoCD (CD GitOps)

* Sync automatique des déploiements
* Mise à jour continue après chaque commit


