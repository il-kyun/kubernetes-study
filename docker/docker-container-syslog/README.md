# docker container syslog

### syslog
- 유닉스 계열 운영체제에서 로그를 수집하는 오래된 표준 중 하나이다.

```
➜  ~ docker run -d --name syslog_container \
> --log-driver=syslog \
> ubuntu:14.04 \
> echo syslogtest
d2ff25a89ad50b2253c160301e5879c6ac042b1612543036a18e3ade52efd74c
docker: Error response from daemon: failed to initialize logging driver: Unix syslog delivery error.
```


