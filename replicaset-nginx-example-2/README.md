
## apply replicaset nginx on pod
1. 레플리카셋이 없지만 라벨이 my-nginx-pods-label 인 포드를 생성한다.
> kubectl apply -f nginx-pod-with-out-rs.yaml
```pod/my-nginx-pod created```

> kubectl get pods --show-labels
```NAME                              READY   STATUS    RESTARTS   AGE   LABELS
hello-minikube-6ddfcc9757-gvbtj   1/1     Running   2          11d   app=hello-minikube,pod-template-hash=6ddfcc9757
my-nginx-pod                      1/1     Running   0          24s   app=my-nginx-pods-label
```

2. 같은 라벨을 가진 레플리카셋을 이용하여 포드를 생성한다. (레플리카셋 포드 생성은 3개)
> kubectl apply -f replicaset-nginx.yaml     
```replicaset.apps/replicaset-nginx created```
> kubectl get pods --show-labels
```
NAME                              READY   STATUS    RESTARTS   AGE     LABELS
hello-minikube-6ddfcc9757-gvbtj   1/1     Running   2          11d     app=hello-minikube,pod-template-hash=6ddfcc9757
my-nginx-pod                      1/1     Running   0          6m53s   app=my-nginx-pods-label
replicaset-nginx-w2pmt            1/1     Running   0          32s     app=my-nginx-pods-label
replicaset-nginx-zdff4            1/1     Running   0          32s     app=my-nginx-pods-label
```

3. 1.에서 생성한 포드를 삭제한다.
> kubectl delete pods my-nginx-pod
```pod "my-nginx-pod" deleted```
> kubectl get pods --show-labels
```
NAME                              READY   STATUS    RESTARTS   AGE     LABELS
hello-minikube-6ddfcc9757-gvbtj   1/1     Running   2          11d     app=hello-minikube,pod-template-hash=6ddfcc9757
replicaset-nginx-b58vz            1/1     Running   0          43s     app=my-nginx-pods-label
replicaset-nginx-w2pmt            1/1     Running   0          2m31s   app=my-nginx-pods-label
replicaset-nginx-zdff4            1/1     Running   0          2m31s   app=my-nginx-pods-label
```

4. 포드를 몇개 만드는지 본다. 
> 마지막에 3개의 포드가 생성되었다. 레플리카셋의 포드 수는 생성될때의 수가 아니라 유지하고자하는 포드의 수이다. 그래서 3개를 유지하기 위해 삭제된 포드가 삭제되자 하나를 더 생성한것이다.
