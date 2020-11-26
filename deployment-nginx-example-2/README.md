# 디플로이먼트 에제

## 실습
레플리카셋 대신 디플로이먼트를 사용해보자.

1. 디플로이먼트로 레플리카셋과 포드 생성한다.  
> kubectl apply -f deployment-nginx.yaml     
```deployment.apps/replicaset-nginx created```
> kubectl get deploy
```
NAME                  READY   UP-TO-DATE   AVAILABLE   AGE
my-nginx-deployment   3/3     3            3           8s
```
> kubectl get replicasets
```
NAME                             DESIRED   CURRENT   READY   AGE
my-nginx-deployment-7484748b57   3         3         3       81s
```
> kubectl get pod
```
NAME                                   READY   STATUS    RESTARTS   AGE
my-nginx-deployment-7484748b57-6pw9k   1/1     Running   0          2m37s
my-nginx-deployment-7484748b57-kd9jg   1/1     Running   0          2m37s
my-nginx-deployment-7484748b57-v6dgf   1/1     Running   0          2m37s
```

확인내용
- 기본 동작은 레플리카셋과 크게 다르지 않아보인다.
