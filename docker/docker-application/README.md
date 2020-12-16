# docker application
- 두개의 컨테이너를 생성한다. 
- mysql container
- wordpress container

### docker run mysql
```
➜  ~ docker run -d \
> --name wordpressdb \
> -e MYSQL_ROOT_PASSWORD=password \
> -e MYSQL_DATABASE=wordpress \
> mysql:5.7
Unable to find image 'mysql:5.7' locally
5.7: Pulling from library/mysql
6ec7b7d162b2: Already exists
fedd960d3481: Already exists
7ab947313861: Already exists
64f92f19e638: Already exists
3e80b17bff96: Already exists
014e976799f9: Already exists
59ae84fee1b3: Already exists
7d1da2a18e2e: Pull complete
301a28b700b9: Pull complete
979b389fc71f: Pull complete
403f729b1bad: Pull complete
Digest: sha256:d4ca82cee68dce98aa72a1c48b5ef5ce9f1538265831132187871b78e768aed1
Status: Downloaded newer image for mysql:5.7
11a062e12e7e38b4ea0d6ccecae93c3f9d72ec9db474d4d0bcf742b2896bfa72
➜  ~ docker ps
CONTAINER ID   IMAGE       COMMAND                  CREATED          STATUS          PORTS                 NAMES
11a062e12e7e   mysql:5.7   "docker-entrypoint.s…"   31 seconds ago   Up 30 seconds   3306/tcp, 33060/tcp   wordpressdb
➜  ~
```

### docker run wordpress
```
➜  ~ docker run -d \
> --name WORDPRESS_DB_PASSWORD=password \
> --name wordpress \
> --link wordpressdb:mysql \
> -p 80 \
> wordpress
Unable to find image 'wordpress:latest' locally
latest: Pulling from library/wordpress
6ec7b7d162b2: Already exists
db606474d60c: Pull complete
afb30f0cd8e0: Pull complete
3bb2e8051594: Pull complete
4c761b44e2cc: Pull complete
c2199db96575: Pull complete
1b9a9381eea8: Pull complete
50450ffc67ee: Pull complete
4d1e5a768e83: Pull complete
5e8be0d1df16: Pull complete
7a6395859d40: Pull complete
7306499d3dce: Pull complete
fa6f0ba15ac6: Pull complete
308a9ead128f: Pull complete
2db781a8732e: Pull complete
63d3161e9e46: Pull complete
a08dd591ed8a: Pull complete
931a26282f2a: Pull complete
f5c6b405e809: Pull complete
caf2bb847f73: Pull complete
Digest: sha256:dadd8e9c2ef6dc2fe146cbc5f2edc0ed8ae1026ae252b52f25791be4d7d16600
Status: Downloaded newer image for wordpress:latest
ccd5006ef265d26374225d625370c84aaaa7f62a6bba6f7dcbc80261f3370a8f
➜  ~ docker ps
CONTAINER ID   IMAGE       COMMAND                  CREATED          STATUS          PORTS                   NAMES
ccd5006ef265   wordpress   "docker-entrypoint.s…"   55 seconds ago   Up 54 seconds   0.0.0.0:55000->80/tcp   wordpress
11a062e12e7e   mysql:5.7   "docker-entrypoint.s…"   3 minutes ago    Up 3 minutes    3306/tcp, 33060/tcp     wordpressdb
➜  ~
```


### -i -t
- -i -t가 컨테이너 내부로 진입하도록 attach 가능한 상태로 설정한다.
```
--interactive , -i		Keep STDIN open even if not attached
--tty , -t		Allocate a pseudo-TTY
```

#### mysql를 -i, -t로 실행한다면?
- mysql 이미지는 컨테이너가 mysqld가 동작하도록 설정돼 있다.
- 그래서 forground로 실행된 로그를 볼 수 있다.
```
➜  ~ docker run -i -t \
> --name mysql_attach_test \
> -e MYSQL_ROOT_PASSWORD=password \
> -e MYSQL_DATABASE=wordpress \
> mysql:5.7
2020-12-16 14:32:39+00:00 [Note] [Entrypoint]: Entrypoint script for MySQL Server 5.7.32-1debian10 started.
2020-12-16 14:32:39+00:00 [Note] [Entrypoint]: Switching to dedicated user 'mysql'
2020-12-16 14:32:39+00:00 [Note] [Entrypoint]: Entrypoint script for MySQL Server 5.7.32-1debian10 started.
2020-12-16 14:32:39+00:00 [Note] [Entrypoint]: Initializing database files
........ <생략> ........
2020-12-16T14:32:49.431086Z 0 [Warning] Insecure configuration for --pid-file: Location '/var/run/mysqld' in the path is accessible to all OS users. Consider choosing a different directory.
2020-12-16T14:32:49.444616Z 0 [Note] Event Scheduler: Loaded 0 events
2020-12-16T14:32:49.445132Z 0 [Note] mysqld: ready for connections.
Version: '5.7.32'  socket: '/var/run/mysqld/mysqld.sock'  port: 3306  MySQL Community Server (GPL)
```

### -d
- -d는 Detached 모드로 컨테이너를 실행한다. 
- Detached 모드는 컨테이너를 백그라운드에서 동작하는 애플리케이션으로써 실행하도록 설정한다. 
- 입출력이 없는 상태로 컨테이너를 실행한다.
```
--detach , -d		Run container in background and print container ID
```

#### ubuntu를 -d로 실행한다면?
- foreground로써 동작하는 프로그램이 없어 컨테이너가 시작되지 않는다.
```
➜  ~ docker run -d --name detach_test ubuntu:14.04
6be967d1dedef9f729f23bf304584d4e9b3a4f82011290e0574f17f627df52c8
➜  ~ docker ps -a
CONTAINER ID   IMAGE          COMMAND                  CREATED          STATUS                      PORTS                   NAMES
c46c8eaff515   ubuntu:14.04   "/bin/bash"              22 hours ago     Exited (0) 24 minutes ago                           mywebserver
➜  ~
```

### -e
- 컨테이너 내부의 환경변수를 설정합니다.

### exec
- -d로 실행한 컨테이너는 attach로 접근할 수 없다.
- exec 명령어로 컨테이너 내부의 셀을 사용할 수 있다.
- -i -t 옵션을 붙이면 상호 입출력이 가능한다. 옵션을 붙이지 않으면 결과만 출력된다.
```
➜  ~ docker exec -i -t wordpressdb /bin/bash
root@11a062e12e7e:/# echo $MYSQL_ROOT_PASSWORD
password
root@11a062e12e7e:/#
```
```
➜  ~ docker exec wordpressdb ls
bin
boot
.....<생략>.....
➜  ~
```

### --link (deprecated)
- --link wordpressdb:mysql : wordpressdb라는 mysql컨테이너와 연결한다.
- 여기서 연결의 의미는 내부IP를 알 필요 없이 접근하는 것을 의미한다.
- wordpress 컨테이너는 wordpressdb의 IP를 몰라도 접근할 수 있다. 
- --link으로 설정된 컨테이너가 실행되 있지 않으면 해당 컨테이너는 실행할 수 없다.
- 도커 브리지(bridge)네트워크를 사용하면 동일한 기능을 사용할 수 있다.
```
--link		Add link to another container
```
```
➜  ~ docker exec wordpress curl mysql:3306 --silent
J
5.7.32	a-T(bca���Aj%t4b%6mysql_native_password!��#08S01Got packets out of order%
➜  ~
```
```
➜  ~ docker stop wordpress wordpressdb
wordpress
wordpressdb
➜  ~ docker start wordpress
Error response from daemon: Cannot link to a non running container: /wordpressdb AS /wordpress/mysql
Error: failed to start containers: wordpress
➜  ~
```

### 바인딩 포트 확인
```
➜  ~ docker port wordpress
80/tcp -> 0.0.0.0:55000
➜  ~
```


















