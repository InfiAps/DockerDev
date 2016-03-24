# DockerDev
## Cheat sheet for Docker tasks - until I get a proper grip on Docker Swarm

Build and tag Docker Image. Change directory to where Dockerfile resides- then use following command
```
docker build -t joegus/joegus-devx .
```

#### create a datacontainer called myslqdata containing /var/lib/mysql
```
docker create -v /var/lib/mysql --name mysqldata joegus/joegus-devx /bin/true
```

#### Start apache on port 41234 and mysql on port 41235 and ssh on port 41233 - IP 192.168.1.2
```
docker run -d -p 41233:22 -p 41234:80 -p 41235:3306 --volumes-from mysqldata joegus/joegus-devx
```
## Backup/Restore database content from/to Docker volume
#### Backup
Mount current directory into the data container named "mysqldata" as /backup (sort of mysqldata:/backup) , then do a tar of mysqldata:/var/lib/mysql and place it in current local directory

```
docker run -d -p 41233:22 -p 41234:80 -p 41235:3306 --volumes-from mysqldata -v $(pwd):/backup joegus/joegus-devx tar cfv  /backup/msqldb.tar /var/lib/mysql
```

#### Restore

```
 docker run --volumes-from mysqldata -v $(pwd):/backup joegus/joegus-devx bash -c "cd /var/lib/mysql && tar xvf /backup/msqldb.tar"
 ```

