# Guía rápida: Sonarqube en Docker

## Dentro de carpeta `jenkins`, crear carpeta `sonar/sonarqube_data`, `sonar/sonarqube_extensions` y `sonar/sonarqube_logs`

```bash
mkdir -p sonar/sonarqube_data sonar/sonarqube_extensions sonar/sonarqube_logs
```

## `(*)opcional` Para saber el id del usuario sonarqube cuando se crea ejecutamos el siguiente comando

```bash
docker run --rm sonarqube:latest id
```
## Dar permisos al usuario de Sonarqube para las carpetas de `sonar`

```bash
sudo chown -R 1000:1000 sonar/
```
## Levantar Sonar

```bash
docker run -d \
  --name sonarqube \
  -p 9000:9000 \
  -v $PWD/sonar/sonarqube_data:/opt/sonarqube/data \
  -v $PWD/sonar/sonarqube_extensions:/opt/sonarqube/extensions \
  -v $PWD/sonar/sonarqube_logs:/opt/sonarqube/logs \
  sonarqube:latest
```

## El usuario y clave de sonar es admin / admin

## Para apagarlo

```bash
docker stop sonarqube
```
