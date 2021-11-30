# TIL_k8s

* pod 생성 
  ```
  pod$ microk8s.kubectl apply -f pod/pod-sample.yaml 
  pod/kubernetes-simple-pod created
  ```

* pod 생성 확인 
  
  ```
  pod$ microk8s.kubectl get replicaset,pods
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

* pod 수정 
  * 레이블 설정 변경 방법을 통해 실행 중인 파드를 재시작하지 않고 실제 서비스에서 분리해 디버깅 용도 등으로 사용할 수 있다.
  ```
  replicaset$ microk8s.kubectl edit pod nginx-replicaset-5g7jc
  pod/nginx-replicaset-5g7jc edited
  ```

* kubectl get -o=jsonpath 을 사용해 파드 전체 내용중 원하는 부분만 선택해서 확인하는 옵션 
  ```
  replicaset$ microk8s.kubectl get pods -o=jsonpath="{range .items[*]}{.metadata.name}{'\t'}{.metadata.labels}{'\n'}{end}"

  nginx-replicaset-5g7jc  map[app:nginx-other]
  nginx-replicaset-c8ccf  map[app:nginx-replicaset]
  nginx-replicaset-cghtl  map[app:nginx-replicaset]
  nginx-replicaset-krmvs  map[app:nginx-replicaset]
  nginx-replicaset-xnx95  map[app:nginx-replicaset]
  nginx-replicaset-zzvjn  map[app:nginx-replicaset]
  ```

# Deployment

* deployment를 클러스터에 적용
  ```
  deployment$ microk8s.kubectl apply -f deployment-nginx.yaml
  deployment.apps/nginx-deployment created
  ```

* deployment 실행 확인 
  ```
    deployment$ microk8s.kubectl get deploy,rs,rc,pods
  ```

  ```
    NAME                               READY   UP-TO-DATE   AVAILABLE   AGE
    deployment.apps/nginx-deployment   3/3     3            3           75s

    ==> nginx-deployment라는 deployment가 생성됨 
  ```  

  ```
    NAME                                          DESIRED   CURRENT   READY   AGE
    replicaset.apps/nginx-deployment-6b4c4b9f48   3         3         3       75s

    ==> deployment.apps/nginx-deployment가 관여하는 replicaset도 생성 
  ```

  ```  
    NAME                                    READY   STATUS    RESTARTS   AGE
    pod/nginx-deployment-6b4c4b9f48-f529b   1/1     Running   0          75s
    pod/nginx-deployment-6b4c4b9f48-vphsr   1/1     Running   0          75s
    pod/nginx-deployment-6b4c4b9f48-x9x6v   1/1     Running   0          75s

    ==> replicaset.apps/nginx-deployment-6b4c4b9f48 인 레플리카세트가 관리하는 pod 들이 생성됨 
  ```
  
  * Kubernetes 네트워크 이해하기 
  https://blog.hyojun.me/8?category=972261 ((1) : 컨테이너 네트워크부터 CNI까지)
  https://blog.hyojun.me/9?category=972261 ((2) : 서비스 개념과 동작 원리)
  
