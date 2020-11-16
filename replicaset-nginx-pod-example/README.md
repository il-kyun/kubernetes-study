
## apply nginx on pod

### 오브젝트 확인
> kubectl api-resources

### my-nginx-pod.yaml을 이용하여 Pod 생성
>  kubectl apply -f my-nginx-pod.yaml

### 생성된 pod 확인
> kubectl get pods

### pod 상세 정보 확인
> kubectl describe pods my-nginx-pod

### pod 삭제
> kubectl delete -f my-nginx-pod.yaml

