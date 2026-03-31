# ETAPE POUR CREATION D'AGENT MAVEN JENKINS AVEC DOCKER


1. **Créer ton propre Docker Maven Jenkins Agent**
2. **Le pousser sur ton Docker Hub**
3. **L’appeler dans ton Jenkinsfile** avec :

   ```groovy
   agent {
     docker {
       image 'nom-image/maven-ton-nom-docker-agent:v1'
       args '--user root -v /var/run/docker.sock:/var/run/docker.sock'
     }
   }
   ```

On y va 👇

---

# 🔥 **1 — Créer ton Dockerfile pour l’agent Maven Jenkins**

Crée un dossier :

```bash
mkdir maven-jenkins-agent
cd maven-jenkins-agent
```

Crée le fichier **Dockerfile** :

```dockerfile
# Base Maven + Java 17
FROM maven:3.9.6-eclipse-temurin-17

USER root

RUN apt-get update && \
    apt-get install -y curl git unzip && \
    curl -fsSL https://download.docker.com/linux/static/stable/x86_64/docker-29.0.3.tgz \
    | tar xz --strip-components=1 -C /usr/local/bin docker/docker

# Optionnel : inclure kubectl / helm plus tard
# RUN curl -LO "https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl"
# RUN chmod +x kubectl && mv kubectl /usr/local/bin/

WORKDIR /home/jenkins

# Garde le conteneur vivant
ENTRYPOINT ["tail", "-f", "/dev/null"]
```

---

# 🚀 **2 — Construire ton image Docker**

Depuis le dossier :

```bash
docker build -t maven-jenkins-agent:v1 .
```

---

# 🌍 **3 — Renommer l’image avec ton Docker Hub**

Supposons que ton Docker Hub est : **donald284**

```bash
docker tag maven-jenkins-agent:v1 donald284/maven-jenkins-agent:v1
```

---

# 📤 **4 — Pousser sur Docker Hub**

Login :

```bash
docker login
```

Push :

```bash
docker push donaldte/maven-jenkins-agent:v1
```

Ton agent est maintenant public ✔️
Tu peux vérifier ici :

👉 [https://hub.docker.com/r/donald284/maven-jenkins-agent](https://hub.docker.com/r/donaldte/maven-jenkins-agent)

---

# 🧩 **5 — Utiliser l’agent dans Jenkins**

Dans ton `Jenkinsfile` :

```groovy
pipeline {
  agent {
    docker {
      image 'donald284/maven-jenkins-agent:v1'
      args '--user root -v /var/run/docker.sock:/var/run/docker.sock'
    }
  }

  stages {

    stage('Build Maven') {
      steps {
        sh 'mvn clean package'
      }
    }

    stage('Docker Build') {
      steps {
        sh 'docker build -t test-app:latest .'
      }
    }
  }
}
```

---

# 🇫🇷 **Explication du paramètre args :**

```groovy
args '--user root -v /var/run/docker.sock:/var/run/docker.sock'
```

* `--user root` → donner les droits Docker au conteneur
* `-v /var/run/docker.sock:/var/run/docker.sock` → utiliser **le Docker de la machine hôte** → indispensable pour build/push Docker

# ajouter jenkins à docker
sudo usermod -a -G docker jenkins


---

# 🎉 **Ton pipeline est maintenant capable de :**

✔️ Builder avec Maven
✔️ Exécuter les tests
✔️ Construire des images Docker
✔️ Pousser les images sur Docker Hub
✔️ Sans installer Maven ou Docker sur Jenkins
✔️ 100% encapsulé dans ton agent Docker personnalisé