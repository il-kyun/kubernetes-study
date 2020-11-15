
## apply replicaset nginx on pod

### 오브젝트 확인
> kubectl api-resources

### replicaset-nginx.yaml을 이용하여 Pod 생성 (3개)
> kubectl apply -f replicaset-nginx.yaml

### 생성된 pod 확인
> kubectl get pods

### replicaset-nginx.yaml을 이용하여 Pod 생성 (4개)
> kubectl apply -f replicaset-nginx-4pods.yaml

### 생성된 pod 확인
> kubectl get pods

### pod 상세 정보 확인
> kubectl describe pods my-nginx-pod

### pod 삭제
> kubectl delete rs replicaset-nginx

