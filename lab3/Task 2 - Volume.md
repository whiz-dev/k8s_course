# Task2 - Volume  

### 유형별 볼륨 생성 및 기능 확인
#  

#### Emptydir Type Volume

1. yaml 확인

```
cat emptydir-pod.yaml
```  

2. yaml 을 활용한 emptydir 유형의 Volume을 사용하는 Pod 생성
```
kubectl create -f emptydir-pod.yaml
```

3. 위 2에서 만든 Pod 내부의 컨테이너 redis로 접속합니다.
```
kubectl exec -it emptydir --container redis -- /bin/bash
```

4. 마운트 된 디렉토리로 이동 후 파일생성
```
cd /mount1
echo hello emptydir >> test.txt
cat test.txt
exit
```

5. 위 2에서 만든 Pod 내부의 컨테이너 nginx로 접속합니다.
```
kubectl exec -it emptydir --container nginx -- /bin/bash
```

6. 디렉토리 이동 후 4에서 생성한 파일 확인
```
cd /mount2
ls
cat test.txt
exit
```
#
#### Hostpath Type Volume

7. yaml 확인
```
cat hostpath-pod.yaml
```  

8. yaml 을 활용한 hostpath 유형의 Volume을 사용하는 Pod 생성
```
kubectl create -f hostpath-pod.yaml
```

9. 위 8 에서 생성한 redis 컨테이너로 접속
```
kubectl exec -it hostpath --container redis -- /bin/bash
```

10. 마운트 된 디렉토리로 이동 후 파일생성
```
cd /mount1
echo hello hostpath >> test.txt
cat test.txt
exit
```

11. 위 8에서 생성한 Pod 가 어떤 노드에 생성되었는지 확인
```
kubectl get pod -o wide
```

12. 위 11에서 확인한 노드의 터미널로 이동하여 파일 생성 확인
```
cd /tmp
ls
cat test.txt
```
#
#### PV Type Volume

13. PV, PVC, Pod yaml 확인
```
cat pv.yaml pvc.yaml pv-pod.yaml
```

14. 위 13에서 확인한 yaml로 리소스 생성
```
kubectl create -f pv.yaml
kubectl create -f pvc.yaml
kubectl create -f pv-pod.yaml
```

15. 생성된 리소스 확인
```
kubectl get pv,pvc,po
```

16. pv-pod 안에 있는 컨테이너로 접속
```
kubectl exec -it persistentvolume --container container -- /bin/bash
```

17. 마운트 된 디렉토리로 이동 후 파일생성
```
cd /mount1
echo hello pv >> test.txt
cat test.txt
exit
```

18. pv-pod 가 어떤 노드에 생성되었는지 확인
```
kubectl get po persistentvolume -o wide
```

19. 위 18에서 확인한 노드의 터미널로 이동하여 파일 생성 확인
```
cd /pv
ls
cat test.txt
```

20. 클리어
```
kubectl delete pod,pv,pvc --all
```
