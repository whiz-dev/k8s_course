# Task 3 - token 방식의 API 접근

### 이번에는 token 방식으로 API 서버에 접근 하는 방식을 학습합니다.
#

1. Namespace 생성
```
kubectl create ns ns1
```
Secret 생성
```
cat <<EOF | kubectl apply -f -
apiVersion: v1
kind: Secret
metadata:
  name: secret1
  namespace: ns1
  annotations:
    kubernetes.io/service-account.name: "default"
type: kubernetes.io/service-account-token
```
2. ns1 의 Service Account, secret 확인
```
kubectl describe -n ns1 serviceaccounts
kubectl describe -n ns1 secret
```  

token 값을 메모장에 저장
```

3. yaml 확인
```
cat tk-pod.yaml
```

4. 위 yaml 로 리소스 생성
```
kubectl create -f tk-pod.yaml
```

5. 저장해둔 token 값을 아래 명령어에 넣고 수행
```
curl -k -H "Authorization: Bearer <TOKEN>" https://<MasterIP>:6443/api/v1
```
```
v1 버전에 대한 api 리스트 정보를 확인가능
```

6.  아래 명령을 수행하여 pod의 정보를 확인
```
curl -k -H "Authorization: Bearer <TOKEN>" https://<MasterIP>:6443/api/v1/namespaces/ns1/pods
```
```
ns1이라는 Namespace의 default 유저(Service Account)에게는 아직 Pod를 조회할 권한이 없기에 Pod정보를 볼 수 없다고 메시지가 출력됨.
```
