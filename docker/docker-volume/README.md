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
```
```

### 도커가 관리하는 볼륨을 생성
```
```











