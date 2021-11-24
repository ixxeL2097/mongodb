# 01 - Dump and Restore MongoDB

## Dump

To dump the database, you can use the following command:

```
mongodump --ssl --sslAllowInvalidCertificates --uri mongodb://$MONGO_USER:$MONGO_PASSWORD@$MONGO_IP:27017/cam?ssl=true --archive=/tmp/mongo_backup.gz --gzip
```

on kubernetes:

```
kubectl -it exec cam-mongo-6bc8b478d-lf4h4 -- bash -c "mongodump --ssl --sslAllowInvalidCertificates --uri mongodb://$MONGO_USER:$MONGO_PASSWORD@$MONGO_IP:27017/cam?ssl=true --archive=/tmp/mongo_backup.gz --gzip"
```

## Restore

You can use a docker-compose.yaml file to import the db :

```yaml
version: '3.8'
services:
  mongodb:
    container_name: mongodb
    image: docker.io/library/mongo
    restart: unless-stopped
    environment:
      - PUID=1000
      - PGID=1000
    volumes:
      - /home/fred/Downloads/mongodb/data:/data/db
      - /home/fred/Downloads/mongodb/resources:/resources
      - /home/fred/Downloads/mongodb/conf:/etc/mongod.conf
    ports:
      - 27017:27017
```

Restore the db:

```console
[root@workstation ~]# docker exec -it mongodb bash
mongorestore mongodb://localhost:27017/cam --archive=/tmp/mongo_backup.gz --gzip -d cam --drop
```

## Install Compass

- https://docs.mongodb.com/compass/current/install/
- https://www.mongodb.com/try/download/compass


