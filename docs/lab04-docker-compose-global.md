# Guía rápida: Integrar Jenkins + Sonarqube + Nexus en Docker-Compose

### Dentro de carpeta `jenkins`, crear carpeta el archivo llamado `docker-compose.yaml`

```bash
vi docker-compose.yaml
```

### colocar el siguiente código en el docker-compose.yaml y luego guardar

```bash

networks:
  ci_net:

services:

  jenkins:
    image: jenkins-docker:latest
    container_name: jenkins
    networks: [ci_net]
    ports:
      - "8080:8080"
      - "50000:50000"
    volumes:
      - ./jenkins_home:/var/jenkins_home
      - /var/run/docker.sock:/var/run/docker.sock
    environment:
      - DOCKER_HOST=unix:///var/run/docker.sock
    group_add:
      - ${DOCKER_GID:-1001}
    restart: unless-stopped
    
  nexus:
    image: sonatype/nexus3:latest
    container_name: nexus
    networks: [ci_net]
    ports:
      - "8081:8081"
      - "8082:8082"
    environment:
      - INSTALL4J_ADD_VM_PARAMS=-Xms2g -Xmx2g -XX:MaxDirectMemorySize=2g
    volumes:
      - ./nexus-data:/nexus-data:Z
    restart: unless-stopped

  sonarqube:
    image: sonarqube:latest
    container_name: sonarqube
    networks: [ci_net]
    ports:
      - "9000:9000"
    volumes:
      - ./sonar/sonarqube_data:/opt/sonarqube/data
      - ./sonar/sonarqube_extensions:/opt/sonarqube/extensions
      - ./sonar/sonarqube_logs:/opt/sonarqube/logs
    healthcheck:
      test: ["CMD", "sh", "-c", "wget -qO- http://127.0.0.1:9000 || exit 1"]
      interval: 20s
      timeout: 5s
      retries: 10
    restart: unless-stopped
```
### Exportar el id del socket de docker

```bash
export DOCKER_GID=$(stat -c '%g' /var/run/docker.sock)
```
### Levantar todos los contenedores

```bash
docker-compose up -d
```
