# docker container fluentd

### fluentd
- 로그 수집 및 저장 기능 제공 오픈소스

### 예제 구성
- docker A, docker B --> fluentd --> mongoDB

### run mongoDB container
```
➜  ~ docker run --name mongoDB -d \
> -p 27017:27017 \
> mongo
➜  ~
```

### run fluentd container
```
➜  ~ docker run -d --name fluentd \
-p 24224:24224 \
-v /Users/parkbobae/project/study/kubernetes-study/docker/docker-container-fluentd/fluent.conf:/fluentd/etc/fluent.conf \
-e FLUENTD_CONF=fluent.conf \
alicek106/fluentd:mongo
a06b3fa729d5771ee06bdc713c617aa501db538dd27dc4472954004ab34126d6
➜  ~
```

### run nginx container
```
➜  ~ docker run -p 80:80 -d \
> --log-driver=fluentd \
> --log-opt fluentd-address=192.168.0.9:24224 \
> --log-opt tag=docker.nginx.webserver \
> nginx
139195db3ba067cca210d68a8851bdcbb826bc991f35685ea1d9e87e6b11bb28
➜  ~
```

### 확인
```
➜  ~ docker exec -it mongoDB mongo
> show dbs
admin   0.000GB
config  0.000GB
local   0.000GB
nginx   0.000GB
> use nginx
switched to db nginx
> show collections
access
> db['access'].find()
{ "_id" : ObjectId("5fe89d8e7c2c48000aab1dba"), "container_id" : "7bd2daa283bd098219db5dfb815c25be75871f750c76766c11cda9823711f0d4", "container_name" : "/heuristic_saha", "source" : "stdout", "log" : "/docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration", "time" : ISODate("2020-12-27T14:43:16Z") }
..... <생략> .....
```