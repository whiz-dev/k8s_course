# Task 4 - 권한관리

### 권한을 부여하여 일정 유저에 대한 접근 제어를 관리합니다.
#

1. yaml 확인
```
cat role-dm.yaml
```

2. 위 yaml 파일로 리소스 생성
```
kubectl create -f role-dm.yaml
```  

3. yaml 확인

```
cat role.yaml
```

4. 위 yaml 파일로 리소스 생성
```
kubectl create -f role.yaml
```  

5. yaml 확인

```
cat role-sa.yaml
```

6. 위 yaml 파일로 리소스 생성
```
kubectl create -f role-sa.yaml
```  
role-sa 의 secret 생성
```
kubectl create -f role-sa-secret.yaml
```

7. role-sa 의 toekn 을 확인
```
kubectl describe secret
```

```
token을 메모장에 저장
```

8. role-sa 유저에 credential 셋팅
```
kubectl config set-credentials role-sa --token=<위에서 저장한 token값>
```

9. 현재 클러스터 명 확인
```
kubectl config get-clusters
```

10. 현재 클러스터를 role-sa 유저가 일정 권한을 가지고 사용할 수 있게 하는 context를 생성
```
kubectl config set-context role-sa-user --cluster=kubernetes --user=role-sa
```

11. yaml 확인
```
cat role-rb.yaml
```

12. 위 yaml파일로 리소스 생성
```
kubectl create -f role-rb.yaml
```

13. context를 변환
```
kubectl config use-context role-sa-user
```

14. pod와 deployment 조회
```
kubectl get pod
kubectl get deploy
```

```
위 단계의 Role 권한에서 pod 의 get 권한은 부여했지만, deploy의 get 권한은 부여하지 않았음.
```

15. context 목록확인
```
kubectl config get-contexts
```

16. context 원상복구
```
kubectl config use-context kubernetes-admin@kubernetes
```
