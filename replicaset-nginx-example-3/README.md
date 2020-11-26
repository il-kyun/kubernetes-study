# 레플리카셋으로 생성된 포드 중 하나의 포드의 라벨을 삭제하면?

## 실습
1. 같은 라벨을 가진 레플리카셋을 이용하여 포드를 생성한다. (레플리카셋 포드 생성은 3개)
> kubectl apply -f replicaset-nginx.yaml     
```replicaset.apps/replicaset-nginx created```
> kubectl get pods --show-labels
```
NAME                              READY   STATUS    RESTARTS   AGE   LABELS
hello-minikube-6ddfcc9757-gvbtj   1/1     Running   2          12d   app=hello-minikube,pod-template-hash=6ddfcc9757
replicaset-nginx-pd7tv            1/1     Running   0          22s   app=my-nginx-pods-label
replicaset-nginx-qmk6j            1/1     Running   0          22s   app=my-nginx-pods-label
replicaset-nginx-t8bpn            1/1     Running   0          22s   app=my-nginx-pods-label
replicaset-nginx-wrln2            1/1     Running   0          22s   app=my-nginx-pods-label

```

2. 1.에서 생성한 포드 중에서 하나의 포드의 라벨을 삭제한다.
> kubectl edit pods replicaset-nginx-pd7tv
```pod/replicaset-nginx-pd7tv edited```
> kubectl get pods --show-labels
```
NAME                              READY   STATUS    RESTARTS   AGE   LABELS
hello-minikube-6ddfcc9757-gvbtj   1/1     Running   2          12d   app=hello-minikube,pod-template-hash=6ddfcc9757
replicaset-nginx-pd7tv            1/1     Running   0          2m    <none>
replicaset-nginx-qmk6j            1/1     Running   0          2m    app=my-nginx-pods-label
replicaset-nginx-t8bpn            1/1     Running   0          2m    app=my-nginx-pods-label
replicaset-nginx-wrln2            1/1     Running   0          2m    app=my-nginx-pods-label
replicaset-nginx-z4xsx            1/1     Running   0          21s   app=my-nginx-pods-label
```

3. 레플리카셋을 삭제한다. 
> kubectl delete rs replicaset-nginx
```replicaset.apps "replicaset-nginx" deleted```
> kubectl get pods --show-labels
```
NAME                              READY   STATUS    RESTARTS   AGE    LABELS
hello-minikube-6ddfcc9757-gvbtj   1/1     Running   2          12d    app=hello-minikube,pod-template-hash=6ddfcc9757
replicaset-nginx-pd7tv            1/1     Running   0          4m8s   <none>

```

4. 확이내용
- 2.에서 라벨을 삭제하였을때 레플리카셋은 새로운 포드를 하나더 생성한다. (레플리카셋 포드 개수 4)
- 3.에서 레플리카셋을 삭제하였을때 라벨이 삭제된 포드는 삭제되지 않는다.
