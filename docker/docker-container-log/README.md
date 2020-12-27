# docker container log

### json-file 로그 사용하기
```
➜  ~ docker run -d --name mysql \
> -e MYSQL_ROOT_PASSWORD=123 \
> mysql:5.7
a68beedce859d452fdc21486dda02824adfa7e3ed3395e419d8e5ce041329256
➜  ~ docker ps
CONTAINER ID   IMAGE          COMMAND                  CREATED          STATUS          PORTS                               NAMES
a68beedce859   mysql:5.7      "docker-entrypoint.s…"   28 seconds ago   Up 28 seconds   3306/tcp, 33060/tcp                 mysql
➜  ~
```
### docker logs 명령어 확인
```
➜  ~ docker logs mysql
2020-12-27 12:54:07+00:00 [Note] [Entrypoint]: Entrypoint script for MySQL Server 5.7.32-1debian10 started.
2020-12-27 12:54:07+00:00 [Note] [Entrypoint]: Switching to dedicated user 'mysql'

.......... <생략> ..........

2020-12-27T12:54:17.011314Z 0 [Note] mysqld: ready for connections.
Version: '5.7.32'  socket: '/var/run/mysqld/mysqld.sock'  port: 3306  MySQL Community Server (GPL)
```
```
➜  ~ docker run -d --name no_passwd_mysql \
mysql:5.7
0d198b0ab1a64788b98631786985203b9ab57d912c0d8b08919a691f92c6bfd3
➜  ~ docker ps
CONTAINER ID   IMAGE          COMMAND                  CREATED         STATUS         PORTS                               NAMES
a68beedce859   mysql:5.7      "docker-entrypoint.s…"   3 minutes ago   Up 3 minutes   3306/tcp, 33060/tcp                 mysql
➜  ~ docker ps -a
CONTAINER ID   IMAGE          COMMAND                  CREATED          STATUS                      PORTS                               NAMES
0d198b0ab1a6   mysql:5.7      "docker-entrypoint.s…"   17 seconds ago   Exited (1) 15 seconds ago                                       no_passwd_mysql
a68beedce859   mysql:5.7      "docker-entrypoint.s…"   3 minutes ago    Up 3 minutes                3306/tcp, 33060/tcp                 mysql
➜  ~ docker start 0d198b0ab1a6
0d198b0ab1a6
➜  ~ docker logs 0d198b0ab1a6
2020-12-27 12:57:50+00:00 [Note] [Entrypoint]: Entrypoint script for MySQL Server 5.7.32-1debian10 started.
2020-12-27 12:57:50+00:00 [Note] [Entrypoint]: Switching to dedicated user 'mysql'
2020-12-27 12:57:50+00:00 [Note] [Entrypoint]: Entrypoint script for MySQL Server 5.7.32-1debian10 started.
2020-12-27 12:57:50+00:00 [ERROR] [Entrypoint]: Database is uninitialized and password option is not specified
You need to specify one of MYSQL_ROOT_PASSWORD, MYSQL_ALLOW_EMPTY_PASSWORD and MYSQL_RANDOM_ROOT_PASSWORD
2020-12-27 12:58:15+00:00 [Note] [Entrypoint]: Entrypoint script for MySQL Server 5.7.32-1debian10 started.
2020-12-27 12:58:15+00:00 [Note] [Entrypoint]: Switching to dedicated user 'mysql'
2020-12-27 12:58:15+00:00 [Note] [Entrypoint]: Entrypoint script for MySQL Server 5.7.32-1debian10 started.
2020-12-27 12:58:15+00:00 [ERROR] [Entrypoint]: Database is uninitialized and password option is not specified
You need to specify one of MYSQL_ROOT_PASSWORD, MYSQL_ALLOW_EMPTY_PASSWORD and MYSQL_RANDOM_ROOT_PASSWORD
➜  ~ docker logs --tail 2 0d198b0ab1a6
2020-12-27 12:58:15+00:00 [ERROR] [Entrypoint]: Database is uninitialized and password option is not specified
	You need to specify one of MYSQL_ROOT_PASSWORD, MYSQL_ALLOW_EMPTY_PASSWORD and MYSQL_RANDOM_ROOT_PASSWORD
➜  ~
```
### log stream으로 확인
```
➜  ~ docker logs -f -t  mysql
2020-12-27T12:54:07.381962800Z 2020-12-27 12:54:07+00:00 [Note] [Entrypoint]: Entrypoint script for MySQL Server 5.7.32-1debian10 started.
2020-12-27T12:54:07.568332800Z 2020-12-27 12:54:07+00:00 [Note] [Entrypoint]: Switching to dedicated user 'mysql'
2020-12-27T12:54:07.573585900Z 2020-12-27 12:54:07+00:00 [Note] [Entrypoint]: Entrypoint script for MySQL Server 5.7.32-1debian10 started.
2020-12-27T12:54:07.637354900Z 2020-12-27 12:54:07+00:00 [Note] [Entrypoint]: Initializing database files
```

```
➜  ~ docker run -i -t --name logstest ubuntu:14.04
root@06cd35314374:/# echo test!
test!
root@06cd35314374:/#
➜  ~ docker logs logstest
root@06cd35314374:/# echo test!
test!
➜  ~
```
### log 크기와 파일 개수 설정
- --log-opt max-size=10k 
- --log-opt max-file=3
```
➜  ~ docker run -it \
> --log-opt max-size=10k --log-opt max-file=3 \
> --name log-test ubuntu:14.04
root@ef599fb0da0f:/#
```

### 드라이버 설정
- --log-driver를 이용하여 드라이버를 설정할 수 있다.
- 기본은 json-file이다.
- syslog, journald, fluentd, awslogs
```
--log-driver=syslog
```



### mac log 저장소
>  ~/Library/Containers/com.docker.docker/Data/log



