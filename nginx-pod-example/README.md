
## apply nginx on pod

### 오브젝트 확인
> kubectl api-resources

### nginx-pod.yaml을 이용하여 Pod 생성
> kubectl apply -f nginx-pod.yaml

### 생성된 pod 확인
> kubectl get pods

### pod 상세 정보 확인
> kubectl describe pods my-nginx-pod

### pod 접근(nginx 호출)
> curl 172.17.0.6

### bash 접근
> kubectl exec -it my-nginx-pod bash

### 접근 로그 확인
> kubectl logs my-nginx-pod
>
### pod 삭제
> kubectl delete -f nginx-pod.yaml

