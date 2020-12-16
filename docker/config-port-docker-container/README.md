# docker container 외부 노출

### 포트 설정 
- -p 옵션으로 port를 설정한다.
- [호스트 포트]:[컨테이너 포트]
```
➜  ~ docker run -i -t --name mywebserver -p 80:80 ubuntu:14.04
root@e9bcf9b24543:/# %
➜  ~ docker ps
CONTAINER ID   IMAGE          COMMAND       CREATED          STATUS          PORTS                NAMES
e9bcf9b24543   ubuntu:14.04   "/bin/bash"   32 seconds ago   Up 31 seconds   0.0.0.0:80->80/tcp   mywebserver
➜  ~

```

### 아이피 + 포트 설정
- 특정 아이피 사용.
```
➜  ~ docker run -i -t --name mywebserver -p 192.168.0.9:7777:80 ubuntu:14.04
root@4d0cc0ae21d1:/# %
➜  ~
```
```
curl 192.168.0.9:7777
<생략>
```

### 아이피 + 포트(여러개) 설정
```
docker run -i -t --name mywebserver -p 192.168.0.9:7777:80 -p 192.168.0.9:7778:80 ubuntu:14.04
```
```
curl 192.168.0.9:7777
<생략>
curl 192.168.0.9:7778
<생략>
```