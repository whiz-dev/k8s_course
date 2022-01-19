# Task 1 - https 방식의 API 접근

### https 방식으로 API 서버에 접근 하는 방식을 학습합니다.
#

1. 실습 디렉토리 이동
```
cd ~/k8s_course/lab7/yaml
```

2. 파일 확인
```
ls
```  

3. config 파일 내용 확인
```
cat ~/.kube/config
```
#

```
3가지 인증서와 API server 주소를 확인 할 수 있습니다.
**인증서 : certificate-authority-data / client-certificate-data / client-key-data**
**API Server 주소 : https://<masterIP>:6443**
```

4. 위에서 확인한 3가지 인증서를 각 변수에 저장.
```
export client=$(grep client-cert ~/.kube/config | cut -d" " -f 6)
export key=$(grep client-key-data ~/.kube/config | cut -d" " -f 6)
export auth=$(grep certificate-authority-data ~/.kube/config | cut -d" " -f 6)
```

5. 저장한 변수 확인
```
echo $client
echo $key
echo $auth
```

6. 3가지 인증서를 Base64 인코딩을 하여 각 파일에 저장
```
echo $client | base64 -di - > ./client.pem
echo $key | base64 -di - > ./client-key.pem
echo $auth | base64 -di - > ./ca.pem
```

7. 저장한 인증서 파일들을 사용, api-server에 https 접근하여 pod 정보를 확인
```
curl --cert ./client.pem --key ./client-key.pem --cacert ./ca.pem https://<MasterIP>:6443/api/v1/pods
```
