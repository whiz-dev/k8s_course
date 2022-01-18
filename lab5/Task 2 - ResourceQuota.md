# Task 2 - ResourseQuota

### Namespace 리소스를 제한
#

1. yaml 확인
```
cat rq-ns.yaml rq1.yaml
```

2. 리소스 생성
```
kubectl create -f rq-ns.yaml
kubectl create -f rq1.yaml
```

3. 생성 확인
```
kubectl get ns
kubectl get resourcequota -n rq-ns
```

4. 상세 정보 조회
```
kubectl describe resourcequota
```

5. pod 생성 yaml 확인
```
cat rq1-pod.yaml
```

6. 리소스 생성
```
kubectl create -f rq1-pod.yaml
```
`rq 제한에 위배되어 생성되지 않음을 확인`


7. 두번째 Pod 생성 yaml 확인
```
cat rq2-pod.yaml
```

8. 리소스 생성
```
kubectl create -f rq2-pod.yaml
```
`정상 생성을 확인`
