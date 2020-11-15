
## apply nginx and unubtu on pod

### 오브젝트 확인
> kubectl api-resources

### nginx-pod-with-ubuntu.yaml을 이용하여 Pod 생성
>  kubectl apply -f nginx-pod-with-ubuntu.yaml

### 생성된 pod 확인
> kubectl get pods

### pod 상세 정보 확인
> kubectl describe pods my-nginx-pod

### linux bash 접근
> kubectl exec -it my-nginx-pod -c ubuntu-sidecar-container bash
> curl localhost
> 리눅스 안에서 localhost로 nginx를 호출한다. (같은 팟 내에서 컨테이너는 localhost로 접근할수 있다.)

### pod 삭제
> kubectl delete -f nginx-pod-with-ubuntu.yaml

