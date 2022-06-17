# Appendix Lab 1. Dashboard


# 설치

```
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.4.0/aio/deploy/recommended.yaml
```


# 토큰 정보 파일들 생성

```
grep 'client-certificate-data' ~/.kube/config | head -n 1 | awk '{print $2}' | base64 -d >> kubecfg.crt

cat kubecfg.crt

grep 'client-key-data' ~/.kube/config | head -n 1 | awk '{print $2}' | base64 -d >> kubecfg.key

cat kubecfg.key

openssl pkcs12 -export -clcerts -inkey kubecfg.key -in kubecfg.crt -out kubecfg.p12 -name "admin-user"
```

# 암호는 1234로 통일
```
chmod a+r kubecfg.p12
ls -la kubecfg.p12

mv kubecfg* /home/ubuntu/

ls /home/ubuntu
```



# AWS cloud9 ubuntu 머신에서 실행
```
scp -i ./k8skey.pem ubuntu@<masterip>:/home/ubuntu/kubecfg* ./
```
# kubecfg* 파일들을 디렉토리 패널에서 윈도우즈로 다운로드


# kubecfg.p12 파일을 더블클릭하여 실행후 다음다음 → pw입력 후 마침

# sa 생성

```
cat <<EOF | kubectl create -f -
apiVersion: v1
kind: ServiceAccount
metadata:
  name: admin-user
  namespace: kubernetes-dashboard
EOF
```
# secret 생성
```
cat <<EOF | kubectl apply -f -
apiVersion: v1
kind: Secret
metadata:
  name: secret-dashboard
  namespace: kubernetes-dashboard
  annotations:
    kubernetes.io/service-account.name: "admin-user"
type: kubernetes.io/service-account-token
EOF
```

# rb 생성

```
cat <<EOF | kubectl create -f -
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: admin-user
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
- kind: ServiceAccount
  name: admin-user
  namespace: kubernetes-dashboard
EOF
```  
# 토큰확인

```
kubectl describe secrets secret-dashboard -n kubernetes-dashboard
```

# 토큰값을 메모장에 저장

# 대시보드 접속 (Chrome)

```
https://<MasterPubicIP>:6443/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/
```

# 안전하지 않음으로 이동 후 위에서 확인한 토큰 입력 로그인
