# docker volume
- 만약 mysql을 사용하다 docker 컨테이너를 삭제하면 데이터도 모두 삭제된다. 데이터를 보구할 수 없다.
- 이를 방지하기 위해서 사용할 수 있는 것으로 volume이 있다.

### volume 활용 방법
- 호스트와 볼륨을 공유
- 볼륨 컨테이너를 활용
- 도커가 관리하는 볼륨을 생성

### 호스트와 볼륨을 공유
- [호스트의 공유 디렉터리]:[컨테이너의 공유 디렉터리]
- -v /Users/parkbobae/docker_volume/wordpress_db:/var/lib/mysql \
- 컨테이너를 삭제해도 데이터는 그대로 남아있다.
- 이미지에 원래 존재하던 디렉터리에 호스트의 볼륨을 공유하면 컨테이너의 디렉터리 자체가 덮어씌워진다.
```
➜  ~ docker run -d \
> --name wordpressdb_hostvolume \
> -e MYSQL_PASSWORD=password \
> -e MYSQL_DATABASE=wordpress \
> -v /Users/parkbobae/docker_volume/wordpress_db:/var/lib/mysql \
> mysql:5.7
23c81a583a381123f24a73bbd1955335e9fa95714c44388a04e8f0cffd6f9ef1
➜  ~ ls /Users/parkbobae/docker_volume/wordpress_db
auto.cnf           ca.pem             client-key.pem     ib_logfile0        ibdata1            mysql              private_key.pem    server-cert.pem    sys
ca-key.pem         client-cert.pem    ib_buffer_pool     ib_logfile1        ibtmp1             performance_schema public_key.pem     server-key.pem     wordpress
➜  ~ 
```
```
➜  ~ docker run -d \
> -e WORDPRESS_DB_PASSWORD=password \
> --name wordpress_hostvolume \
> --link wordpressdb_hostvolume:mysql \
> -p 80 \
> wordpress
3d3be2603835122dab613738ee25b6a4e16daf435825470b190f30b1a0456422
➜  ~
```
```
➜  ~ docker stop 3d3be2603835 23c81a583a38
3d3be2603835
23c81a583a38
➜  ~ docker rm 3d3be2603835 23c81a583a38
3d3be2603835
23c81a583a38
➜  ~ ls /Users/parkbobae/docker_volume/wordpress_db
auto.cnf           ca.pem             client-key.pem     ib_logfile0        ibdata1            performance_schema public_key.pem     server-key.pem     wordpress
ca-key.pem         client-cert.pem    ib_buffer_pool     ib_logfile1        mysql              private_key.pem    server-cert.pem    sys
➜  ~
```

```
➜  ~ docker run -i -t --name volume_dummy alicek106/volume_test
Unable to find image 'alicek106/volume_test:latest' locally
.....<생략>.....
Status: Downloaded newer image for alicek106/volume_test:latest
root@95d5c9423b80:/# ls /home/testdir_2
test
```
```
➜  ~ docker run -i -t \
> --name volume_overide \
> -v /Users/parkbobae/docker_volume/wordpress_db:/home/testdir_2 \
> alicek106/volume_test
root@4a485451ccf1:/# ls /home/testdir_2
auto.cnf    ca.pem           client-key.pem  ib_logfile0  ibdata1  performance_schema  public_key.pem   server-key.pem  wordpress
ca-key.pem  client-cert.pem  ib_buffer_pool  ib_logfile1  mysql    private_key.pem     server-cert.pem  sys
root@4a485451ccf1:/#
```

### -v
- -v 옵션은 단일 파일도 가능하다.
- ex) -v /home/hello:/hello - -v /home/hello2:/hello2


### 볼륨 컨테이너
- --volumes-from 옵션으로 -v 옵션으로 적용된 컨테이너의 볼륨 컨테이너를 공유하는 것 입니다.
- 여러 개의 컨테이너가 동일한 컨테이너에 --volumes-from 옵션을 사용해서 볼륨을 공유해 사용할 수 있습니다.

```
➜  ~ docker run -i -t \
--name volumes_from_container \
> --volumes-from volume_overide \
> ubuntu:14.04
root@c6434efeb9ca:~# ls /home/testdir_2/
auto.cnf    ca.pem           client-key.pem  ib_logfile0  ibdata1  performance_schema  public_key.pem   server-key.pem  wordpress
ca-key.pem  client-cert.pem  ib_buffer_pool  ib_logfile1  mysql    private_key.pem     server-cert.pem  sys
root@c6434efeb9ca:~#

```

### 도커가 관리하는 볼륨을 생성
- docker 자체에서 관리하는 volume 기능이다.
- 플러그인 드라이버를 설정하면 여러 종류의 스토리지 백엔드를 쓸 수 있다.
- -v [볼륨의 이름]:[컨테이너의 공유 디렉터리]
```
➜  ~ docker volume create --name myvolume
myvolume

➜  ~ docker volume ls
DRIVER    VOLUME NAME
local     myvolume

➜  ~ docker run -i -t --name myvolume_1 \
-v myvolume:/root/ \
ubuntu:14.04
root@abe355a38860:/# echo hello, volume! >> /root/volume
root@abe355a38860:/# exit
exit

➜  ~ docker run -i -t --name myvolume_2 \
-v myvolume:/root/ \
ubuntu:14.04
root@5ee9484e15f9:/# cat /root/volume
hello, volume!
root@5ee9484e15f9:/#
```
- 볼륨 위치 확인
```
➜  ~ docker inspect --type volume myvolume
[
    {
        "CreatedAt": "2020-12-18T15:01:02Z",
        "Driver": "local",      # 볼륨이 사용하는 드라이버
        "Labels": {},           # 볼륨을 구분하는 라벨
        "Mountpoint": "/var/lib/docker/volumes/myvolume/_data",
        "Name": "myvolume",
        "Options": {},
        "Scope": "local"
    }
]
➜  ~
```

```
➜  ~ docker run -i -t --name volume_auto \
> -v /root \
> ubuntu:14.04
root@2b64107909b5:/#


➜  ~ docker volume ls
DRIVER    VOLUME NAME
local     c45db738e8eac2c3a61297592d5d06eb1812c50506eb313c5cf847daaf52895e

➜  ~ docker container inspect volume_auto
[
    {
        ... <생략> ...
        "Mounts": [
            {
                "Type": "volume",
                "Name": "c45db738e8eac2c3a61297592d5d06eb1812c50506eb313c5cf847daaf52895e",
                "Source": "/var/lib/docker/volumes/c45db738e8eac2c3a61297592d5d06eb1812c50506eb313c5cf847daaf52895e/_data",
                "Destination": "/root",
                "Driver": "local",
                "Mode": "",
                "RW": true,
                "Propagation": ""
            }
        ]
        ... <생략> ...
    }
]
➜  ~
```

### 볼륨 삭제
```
➜  ~ docker volume prune
WARNING! This will remove all local volumes not used by at least one container.
Are you sure you want to continue? [y/N] y
Total reclaimed space: 0B
➜  ~
```

### -v 옵션 대신 --mount 옵션 사용
- volume을 사용한 경우
- type : volume
- source에 볼륨명 입력
```
➜  ~ docker run -i -t --name mount_option_1 \
> --mount type=volume,source=myvolume,target=/root \
> ubuntu:14.04
root@33d72b16e10b:/# exit
exit
➜  ~ docker volume ls
DRIVER    VOLUME NAME
local     myvolume
➜  ~
```
- 호스트 디렡거리를 컨테이너 내부에 마운트 하는 경우
- type : bind
- source에 디렉터리 주소 입력
```
➜  docker_volume docker run -i -t --name mount_option_2 \
--mount type=bind,source=/Users/parkbobae/docker_volume,target=/home/testdir \
ubuntu:14.04
root@2bc612f0419b:/# exit
```









