# Task 3 - token 방식의 API 접근

### 이번에는 token 방식으로 API 서버에 접근 하는 방식을 학습합니다.
#

1. Namespace 생성
```
kubectl create ns ns1
```

2. ns1 의 Service Account 와 Secret을 확인
```
kubectl describe -n ns1 serviceaccounts
kubectl describe -n ns1 secrets
```  
```
token 값을 메모장에 저장
```

3.
