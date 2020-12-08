# 서비스 에제

## 실습
1. 서비스 실행  
> kubectl apply -f hostname-svc-clusterip.yaml     
```service/hostname-svc-clusterip created```
> kubectl get services
> kubectl get svc
```
NAME                     TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
hostname-svc-clusterip   ClusterIP   10.101.53.50    <none>        8080/TCP         30s
kubernetes               ClusterIP   10.96.0.1       <none>        443/TCP          33d
```
> kubectl run -i --tty --rm debug --image=alicek106/ubuntu:curl --restart=Never -- bash
- 서비스의 IP와 포트를 통해 포드에 접근 할 수 있다.
- 자동으로 요청이 분산된다.
```
If you don't see a command prompt, try pressing enter.
root@debug:/# curl 10.101.53.50:8080 --silent | grep Hello
        <p>Hello,  hostname-deployment-7dfd748479-qbp22</p>     </blockquote>
root@debug:/# curl 10.101.53.50:8080 --silent | grep Hello
        <p>Hello,  hostname-deployment-7dfd748479-dpv7l</p>     </blockquote>
root@debug:/# curl 10.101.53.50:8080 --silent | grep Hello
        <p>Hello,  hostname-deployment-7dfd748479-sztcq</p>     </blockquote>
```
- DNS를 사용할 수 있다.
```
root@debug:/# curl hostname-svc-clusterip:8080 --silent | grep Hello
        <p>Hello,  hostname-deployment-7dfd748479-dpv7l</p>     </blockquote>
```

> 서비스 접근 명령어 --- port:8080 ---> ClusterIP 타입 서비스 --- port:80 ---> 포드 1....N
 