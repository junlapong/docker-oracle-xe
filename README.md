# docker-oracle-xe

## 1. Build Docker Image

```
$ hub clone oracle/docker-images

$ cd docker-images/OracleDatabase/SingleInstance/dockerfiles

$ ls
11.2.0.2            12.2.0.1            18.4.0              buildDockerImage.sh
12.1.0.2            18.3.0              19.3.0
```

Download [oracle-xe-11.2.0-1.0.x86_64.rpm.zip](https://drive.google.com/file/d/1jtTIuey94tIYpdapNUq11ix3EaKfsOv1/view)

Then copy Oracle XE setup file to the folder matched with the version you downloaded. In this example, copy the setup file to `11.2.0.2` folder.

Build the image with `buildDockerImage.sh` script. For Unix users, you may need prefix sudu command or allow execution permission to `buildDockerImage.sh` script.

```
$ ./buildDockerImage.sh -v 11.2.0.2 -x

$ docker image ls oracle/database                                  
REPOSITORY                       TAG                     IMAGE ID            CREATED             SIZE
oracle/database                  11.2.0.2-xe             e1c1948b87b0        12 seconds ago      1.13GB
```

## 2. Run Docker

Run with docker command

```
$ docker run --name OracleXE \
         --shm-size=1g \
         -p 1521:1521 \
         -p 8081:8080 \
         -e ORACLE_PWD=12345 \
         -v ./data:/u01/app/oracle/oradata \
         oracle/database:11.2.0.2-xe
```

Or run with Docker Compose

Example `docker-compose.yml`

```yml
version: '3.3'

services:
    OracleXE:
        container_name: OracleXE
        image: 'oracle/database:11.2.0.2-xe'
        shm_size: '1gb'
        ports:
            - '1521:1521'
            - '8081:8080'
        environment:
            - ORACLE_PWD=12345
        volumes:
            - ./data:/u01/app/oracle/oradata
```

```
$ docker-compose up -d
$ docker-compose logs -f
```

### Test Connection

Check listening post `1521`

```
$ telnet localhost 1521
Trying 127.0.0.1...
Connected to localhost.
Escape character is '^]'.
```

Create a new Oracle connection with [DBeaver](https://dbeaver.io/) and fill the following information.

| Name | Value |
|-----:|-------|
| Host | localhost |
| Port | 1521 |
| Database (SID) | XE |
| Username | SYS |
| Role | SYSDBA |
| Password | 12345 |

### Test SQL query

```sql
SELECT SYSDATE FROM DUAL;
```

## References

- https://github.com/oracle/docker-images
- [Set up Oracle XE with Docker container and connect with DBeaver](https://www.codesanook.com/setup-oracle-xe-database-on-docker-container-and-connect-with-dbeaver)
