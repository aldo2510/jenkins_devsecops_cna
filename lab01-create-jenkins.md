# Guía rápida: Jenkins en Docker

## Crear carpeta `jenkins`

```bash
mkdir jenkins
```

## Crear archivo `Dockerfile` dentro de la carpeta `jenkins`

```bash
vi Dockerfile
```

### Pegar este contenido en `Dockerfile`

```dockerfile
FROM jenkins/jenkins:lts-jdk17
USER root
RUN apt-get update && apt-get install -y \
      ca-certificates curl gnupg lsb-release git \
 && install -m 0755 -d /etc/apt/keyrings \
 && curl -fsSL https://download.docker.com/linux/debian/gpg | gpg --dearmor -o /etc/apt/keyrings/docker.gpg \
 && echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/debian \
    $(. /etc/os-release && echo \"$VERSION_CODENAME\") stable" > /etc/apt/sources.list.d/docker.list \
 && apt-get update && apt-get install -y docker-ce-cli docker-compose-plugin \
 && apt-get clean && rm -rf /var/lib/apt/lists/*
USER jenkins
EXPOSE 8080 50000
```

## Compilar la imagen

```bash
docker build -t jenkins-docker:latest .
```

## Dentro de la carpeta `jenkins`, crear carpeta `jenkins_home`

```bash
mkdir jenkins_home
```

## Dar permisos al usuario de Jenkins para `jenkins_home`

```bash
sudo chown -R 1000:1000 ./jenkins_home
```

## Levantar Jenkins

```bash
docker run -d \
  --name jenkins \
  -p 8080:8080 -p 50000:50000 \
  -v $PWD/jenkins_home:/var/jenkins_home \
  -v /var/run/docker.sock:/var/run/docker.sock \
  --group-add $(stat -c '%g' /var/run/docker.sock) \
  -e DOCKER_HOST=unix:///var/run/docker.sock \
  --restart unless-stopped \
  jenkins-docker:latest

```

## Para apagarlo

```bash
docker stop jenkins
```
