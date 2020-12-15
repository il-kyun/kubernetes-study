# docker container 생성

### docker version 확인
```
➜  ~ docker -v
Docker version 20.10.0, build 7287ab3
```

### docker container 실행(?)
- docker run = docker pull + create + start + attach
- local에 image가 없다면 docker hub에서 가져온다.
- -i 옵션 : 상호 입출력, -t 옵션 : tty를 활성화 -> bash shell 사용 가능.
- exit, ctrl + D 명령어는 컨테이너에서 나오면서 컨테이너를 stop한다.
```
➜  ~ docker run -i -t ubuntu:14.04
Unable to find image 'ubuntu:14.04' locally ## 로컬에 이미지가 없어요.
14.04: Pulling from library/ubuntu          ## 허브에서 가져오겠다!!
2e6e20c8e2e6: Pull complete
95201152d9ff: Pull complete
5f63a3b65493: Pull complete
Digest: sha256:63fce984528cec8714c365919882f8fb64c8a3edf23fdfa0b218a2756125456f
Status: Downloaded newer image for ubuntu:14.04
root@ce35478a798b:/# ls
bin  boot  dev  etc  home  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
root@ce35478a798b:/# exit
exit
➜  ~ docker ps -a
CONTAINER ID   IMAGE                                 COMMAND                  CREATED             STATUS                       PORTS                               NAMES
ce35478a798b   ubuntu:14.04                          "/bin/bash"              2 minutes ago       Exited (0) 11 seconds ago                                        silly_kepler
➜  ~
```

### docker container pull
- pull : image download
```
➜  ~ docker pull centos:7
7: Pulling from library/centos
2d473b07cdd5: Pull complete
Digest: sha256:0f4ec88e21daf75124b8a9e5ca03c37a5e937e0e108a255d890492430789b60e
Status: Downloaded newer image for centos:7
docker.io/library/centos:7
➜  ~ docker images
REPOSITORY                    TAG       IMAGE ID       CREATED        SIZE
centos                        7         8652b9f0cb4c   4 weeks ago    204MB
ubuntu                        14.04     df043b4f0cf1   2 months ago   197MB
➜  ~ docker ps -a
CONTAINER ID   IMAGE                                 COMMAND                  CREATED             STATUS                       PORTS                               NAMES
ce35478a798b   ubuntu:14.04                          "/bin/bash"              5 minutes ago       Exited (0) 3 minutes ago                                         silly_kepler
➜  ~
```

### docker container create
- create : image로 이용해서 container를 생성한다.
- 이미지가 없다면 pull을 실행한다.
```
➜  ~ docker create -i -t --name mycentos centos:7
c149554a6a071f67d02cb4425cd1ac54042d1df4a95ba9e76d00ca54fc786f3b
➜  ~ docker ps -a
CONTAINER ID   IMAGE                                 COMMAND                  CREATED         STATUS                       PORTS                               NAMES
c149554a6a07   centos:7                              "/bin/bash"              8 seconds ago   Created                                                          mycentos
ce35478a798b   ubuntu:14.04                          "/bin/bash"              6 minutes ago   Exited (0) 4 minutes ago                                         silly_kepler
```

### docker container start
- start : 실행한다.
```
➜  ~ docker start mycentos
mycentos
➜  ~ docker ps -a
CONTAINER ID   IMAGE                                 COMMAND                  CREATED          STATUS                       PORTS                               NAMES
c149554a6a07   centos:7                              "/bin/bash"              53 seconds ago   Up 8 seconds                                                     mycentos
ce35478a798b   ubuntu:14.04                          "/bin/bash"              7 minutes ago    Exited (0) 5 minutes ago                                         silly_kepler
➜  ~
```

### docker container attach
- attach : container 내부로 들어간다.
- Ctrl + P, Q container를 정지하지 않고 빠져나온다.
```
➜  ~ docker attach mycentos
[root@c149554a6a07 /]# read escape sequence
➜  ~ docker ps -a
CONTAINER ID   IMAGE                                 COMMAND                  CREATED         STATUS                        PORTS                               NAMES
c149554a6a07   centos:7                              "/bin/bash"              3 minutes ago   Up 2 minutes                                                      mycentos
ce35478a798b   ubuntu:14.04                          "/bin/bash"              9 minutes ago   Exited (0) 7 minutes ago                                          silly_kepler
```


