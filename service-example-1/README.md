# 서비스 에제

### 서비스란?   
> 포트를 외부로 노출해 사용자들이 접근하거나, 다른 디플로이먼트의 포트들이 내부적으로 접근하는데 사용하는 쿠버네티스 오브젝트.
- 여러 개의 포드에 쉽게 접근할 수 있도록 고유한 도메인 이름을 부여합니다.
- 여러 개의 포드에 접근할 때, 요청을 분산하는 로드 밸런서 기능을 수행합니다.
- 클라우드 플랫폼의 로드 밸런서, 클러스터 노드의 포트 등을 통해 포드를 외부로 노출합니다.

### 서비스 타입 3가지
- ClusterIP 타입 : 쿠버네티스 내부에서만 포드들에 접근할 때 사용합니다. 외부로 포드를 노출하지 않기 때문에 쿠버네티스 클러스터 내부에서만 사용되는 포드에 적합합니다.
- NodePort 타입 : 포드에 접근할 수 있는 포트를 클러스터의 모든 노드에 동일하게 개방합니다. 외부에서 포드에 접근할 수 있는 서비스 타입입니다. 
- LoadBalancer 타입 : 클라우드 플랫폼에서 제공하는 로드 밸런서를 동적으로 프로비저닝해 포드에 연결합니다. 외부에서 포드에 접근 할 수 있다. AWS, GCP와 같은 클라우드 환경에서 사용할 수있다.

## 실습
1. 디플로이먼트로 레플리카셋과 포드 생성한다.  
> kubectl apply -f deployment-hostname.yaml     
```deployment.apps/hostname-deployment created```
> kubectl get deploy
```
NAME                  READY   UP-TO-DATE   AVAILABLE   AGE
hostname-deployment   3/3     3            3           29s
```
> kubectl get replicasets
```
NAME                             DESIRED   CURRENT   READY   AGE
hostname-deployment-7dfd748479   3         3         3       46s
```
> kubectl get pod -o wide
```
NAME                                   READY   STATUS    RESTARTS   AGE    IP           NODE       NOMINATED NODE   READINESS GATES
hostname-deployment-7dfd748479-dpv7l   1/1     Running   0          103s   172.17.0.6   minikube   <none>           <none>
hostname-deployment-7dfd748479-qbp22   1/1     Running   0          103s   172.17.0.4   minikube   <none>           <none>
hostname-deployment-7dfd748479-sztcq   1/1     Running   0          103s   172.17.0.7   minikube   <none>           <none>
```

### 클러스터의 노드 중 하나에 접속해, 노드에서 curl을 통해 포드로 접근해도 됩니다.
> kubectl run -i --tty --rm debug --image=alicek106/ubuntu:curl --restart=Never curl 172.17.0.6 | grep Hello
```
        <p>Hello,  hostname-deployment-7dfd748479-dpv7l</p>     </blockquote>
```
