# LoadBalancer Type 서비스 에제
- LoadBalancer 타입의 서비스는 서비스 생성과 동시에 로드 밸런서를 새롭게 생성해 포드와 연결한다.
- 로드 밸런서를 동적으로 생성하는 기능을 제공하는 환경에서만 사용할 수 있다.
- NodePort는 IP를 알아야 포드에 접근할 수 있지만 LoadBalancer는 클라우드 플랫폼으로부터 도메인 이름과 IP를 할당받기 때문에 NodePort보다 더욱 쉽게 포드에 접근할 수 있다. 

## 실습
....불가....
1. 서비스 실행  
> kubectl apply -f hostname-svc-lb.yaml     
```service/hostname-svc-lb created```
> kubectl get svc
- < pending > 부분은 클라우드 플랫폼에서 할당하게 된다.
- 80:32578/TCP 32578 포트가 개발되어 있어 NodePort 서비스로 접근할 수 있다.
```
NAME                    TYPE           CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
hostname-svc-lb         LoadBalancer   10.110.98.180   <pending>     80:32578/TCP     4m55s
kubernetes              ClusterIP      10.96.0.1       <none>        443/TCP          33d
```



### 확인내용
- AWS로 실습을 다시하자.
