# Guía rápida: Nexus en Docker

## Dentro de carpeta `jenkins`, crear carpeta `nexus-data`

```bash
mkdir nexus-data
```

## Dar permisos al usuario de Nexus para `nexus-data`

```bash
sudo chown -R 200:200 ./nexus-data
```

## Levantar Nexus

```bash
docker run -d \
  --name nexus \
  -p 8081:8081 \
  -p 8082:8082 \
  -e INSTALL4J_ADD_VM_PARAMS="-Xms2g -Xmx2g -XX:MaxDirectMemorySize=2g" \
  -v $PWD/nexus-data:/nexus-data:Z \
  sonatype/nexus3:latest
```

## Obtener clave de usuario admin de nexus

```bash
docker exec nexus cat /nexus-data/admin.password
```


## Para apagarlo

```bash
docker stop nexus
```
