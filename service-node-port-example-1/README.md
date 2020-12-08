# NodePort Type 서비스 에제
- NodePort 타입의 서비스는 클러스터 외부에서도 접근할 수 있다.
- 모든 Node의 특정 Port를 개방해 서비스에 접근하는 방식이다. 

## 실습
1. 서비스 실행  
> kubectl apply -f hostname-svc-nodeport.yaml     
```service/hostname-svc-nodeport created```
> kubectl get services
> kubectl get svc
- 8080:30340/TCP : 330340은 모든 노드에서 동일하게 접근할 수 있는 포트를 의미한다.
```
NAME                     TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
hostname-svc-nodeport    NodePort    10.99.93.116    <none>        8080:30340/TCP   25s
kubernetes               ClusterIP   10.96.0.1       <none>        443/TCP          33d
```
> kubectl get nodes -o wide
- minikube라 실습 불가
- 클러스터 구성이 필요하다. 
```
NAME       STATUS   ROLES    AGE   VERSION   INTERNAL-IP    EXTERNAL-IP   OS-IMAGE           KERNEL-VERSION    CONTAINER-RUNTIME
minikube   Ready    master   33d   v1.19.2   192.168.49.2   <none>        Ubuntu 20.04 LTS   5.4.39-linuxkit   docker://19.3.8
```
> curl <INTERNAL-IP>:30340 --silent | grep Hello

### GKE, AWS 클라우드 환경.
- NodePort 통신을 위해서는 방화벽 설정 또는 Security Group에 별도의 Inbound 규칙을 추가해야한다.


### 내부에서 접근해보자.
> kubectl run -i --tty --rm debug --image=alicek106/ubuntu:curl --restart=Never --bash
```
root@bash:/# curl 10.99.93.116:8080 --silent | grep Hello
        <p>Hello,  hostname-deployment-7dfd748479-qbp22</p>     </blockquote>
root@bash:/# curl hostname-svc-nodeport:8080 --silent | grep Hello
        <p>Hello,  hostname-deployment-7dfd748479-qbp22</p>     </blockquote>
```

### 확인내용
- NodePort 서비스는 내부 네트워크와 외부 네트워크 양쪽에서 접근할 수 있다.
- 실제 운영 환경에서 NodePort로 서비스를 외부에 제공하는 경우는 많지 않다.
- 이유 : 포트 번호 80, 443으로 설정하기 적절하지 않고 SSL 인증서 적용, 라우팅 등과 같은 복잡한 설정을 서비스하기 어렵다.
- 이러한 이유로 Ingress라는 쿠버네티스 오브젝트에서 간접적으로 사용되는 경우가 많다.
- 인그레스는 LoadBalancer + NodePort 정도로 이해할 수 있다.