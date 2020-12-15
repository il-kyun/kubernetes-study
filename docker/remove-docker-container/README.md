# docker container 삭제

### docker rm
```
➜  ~ docker ps
CONTAINER ID   IMAGE      COMMAND                  CREATED          STATUS          PORTS                               NAMES
c149554a6a07   centos:7   "/bin/bash"              33 minutes ago   Up 32 minutes                                       mycentos
0943fbd428b7   mysql      "docker-entrypoint.s…"   3 days ago       Up 3 days       0.0.0.0:3306->3306/tcp, 33060/tcp   bbb-db-mysql
➜  ~ docker rm mycentos
Error response from daemon: You cannot remove a running container c149554a6a071f67d02cb4425cd1ac54042d1df4a95ba9e76d00ca54fc786f3b. Stop the container before attempting removal or force remove
➜  ~ docker stop mycentos
mycentos
➜  ~ docker rm mycentos
mycentos
```

### docker container prune
```
➜  ~ docker container prune
WARNING! This will remove all stopped containers.
Are you sure you want to continue? [y/N] y
Deleted Containers:
ce35478a798b8fcf7b301cd40f54cf7466cb5bbe9155fdaed612888e321db0bb
7269986bfc331a3b9b5f04e5fec1415fec63a8e11740c62f7ac3e158933daad7

Total reclaimed space: 3.355MB
```

### docker stop $(docker ps -a -q) | docker rm $(docker ps -a -q)
- -a는 전체 목록 -q ID만 출력
```
➜  ~ docker ps -a
CONTAINER ID   IMAGE     COMMAND                  CREATED      STATUS      PORTS                               NAMES
0943fbd428b7   mysql     "docker-entrypoint.s…"   3 days ago   Up 3 days   0.0.0.0:3306->3306/tcp, 33060/tcp   bbb-db-mysql
➜  ~ docker stop $(docker ps -a -q)
0943fbd428b7
➜  ~ docker rm $(docker ps -a -q)
0943fbd428b7
➜  ~ docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
➜  ~
```