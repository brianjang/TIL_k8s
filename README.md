# TIL_k8s

* pod 생성 
  ```
  pod$ microk8s.kubectl apply -f pod/pod-sample.yaml 
  pod/kubernetes-simple-pod created
  ```

* pod 생성 확인 
  
  ```pod$ microk8s.kubectl get replicaset,pods
  NAME                    READY   STATUS    RESTARTS   AGE
  kubernetes-simple-pod   1/1     Running   0          7m43s
  ```


* 현재 pod 생명 주기 확인 
  ```
  p od$ microk8s.kubectl describe pod kubernetes-simple-pod
  (결과중 Status와 Conditions 확인)
  ```
  
* pod 삭제 
  ```
  pod$ microk8s.kubectl delete pod kubernetes-simple-pod
  pod "kubernetes-simple-pod" deleted
  ```
  
# Controller

* --cascade=false 옵션으로 레플리카세트 삭제시 파드는 남겨두고 컨트롤러만 삭제
  ```
  replicaset$ microk8s.kubectl delete replicaset nginx-replicaset --cascade=false
  replicaset.apps "nginx-replicaset" deleted
```

