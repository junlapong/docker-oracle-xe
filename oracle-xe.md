# Oracle XE

__Step 1__

```sh
git clone https://github.com/oracle/docker-images
```

__Step 2__

```sh
# Oracle v18.4.0 ###
cd docker-images/OracleDatabase/SingleInstance/dockerfiles/18.4.0

# OR
# Oracle v21.3.0 ###
cd docker-images/OracleDatabase/SingleInstance/dockerfiles/21.3.0
```

__Step 3__

```sh
### Build ###
docker build -t docker.io/oracle-xe:21.3.0 -f Dockerfile.xe .
```

__Step 4__

```sh
docker run --name OracleXE \
--rm -ti \
-p 1521:1521 \
-p 5500:5500 \
-e ORACLE_PWD=Pa55w0rd \
-e ORACLE_CHARACTERSET=TH8TISASCII \
docker.io/oracle-xe:21.3.0
```

```sh
docker exec -it OracleXE sqlplus system/Pa55w0rd@xe
```

## Docker Compose

```sh
docker-compose -f docker-compose.yml up

# OR
docker-compose exec oracle-xe bash -c "sqlplus / as sysdba"

# OR
docker-compose exec oracle-xe sqlplus system/Pa55w0rd@xe
```

```
SQL*Plus: Release 21.0.0.0.0 - Production on Sat Jul 1 09:53:45 2023
Version 21.3.0.0.0

Copyright (c) 1982, 2021, Oracle.  All rights reserved.


Connected to:
Oracle Database 21c Express Edition Release 21.0.0.0.0 - Production
Version 21.3.0.0.0

SQL> SELECT SYSDATE FROM DUAL;

SYSDATE
---------
01-JUL-23
```

### Notes

- [Run SQL startup script on Oracle database container initialization](https://faun.pub/run-sql-startup-script-on-oracle-database-container-initialization-e94ea983dccd)
