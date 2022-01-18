# Task 1 - Daemonset  

### 또 다른 컨트롤러인 Daemonset의 동작방식과 기능들을 확인
#

1. 실습 디렉토리 이동
```
cd ~/k8s_course/lab4/yaml
```

2. Daemonset 조회
```
kubectl get daemonset
```  

3. yaml 확인
```
cat daemonset1.yaml
```

4. daemonset 생성
```
kubectl create -f daemonset1.yaml
```

5. 생성된 리소스 확인
```
kubectl get daemonset
kubectl get pod
```

6. 생성된 각 Pod 가 각 워커노드에 생성되었는지 확인
```
kubectl get pod -o wide
kubectl describe pod
```

7. nodeselector 기능을 확인하기위해 각 워커노드에 labeling
```
kubectl label nodes k8s-worker1 os=centos
kubectl label nodes k8s-worker2 os=ubuntu
```

8. 두번째 Daemonset yaml 확인
```
cat daemonset2.yaml
```

9. daemonseet 생성
```
kubectl create -f daemonset2.yaml
```

10. 생성된 리소스 확인
```
kubectl get daemonset
kubectl get pod
```

11. 생성된 각 Pod 어떤 워커노드에 생성되었는지 확인
```
kubectl get pod -o wide
kubectl describe pod
```

12. 워커노드에 설정했던 Label 삭제
```
kubectl label nodes k8s-worker1 os-
kubectl label nodes k8s-worker2 os-
```

13. clear
```
kubectl delete daemonset --all
```
