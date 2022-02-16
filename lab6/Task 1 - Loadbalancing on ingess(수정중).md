# Task 1 - Loadbalancing on ingess

### Ingress 의 구현체인 Contorller 설치와 로드밸런싱 실습
#

1. 실습 디렉토리 이동
```
cd ~/k8s_course/lab6/yaml
```

2. 웹브라우저로 아래 링크에 접속하여 ingress controller 설치 전 확인
```
https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.0.0/deploy/static/provider/baremetal/deploy.yaml
```

3. nginx ingress controller 설치
```
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.0.0/deploy/static/provider/baremetal/deploy.yaml
```  

4. 홈페이지에 관한 Service, Pod yaml 확인
```
cat homepage.yaml
```

5. 위 yaml 로 리소스 생성
```
kubectl create -f homepage.yaml
```

6. 고객센터에 관한 Service, Pod yaml 확인
```
cat customer.yaml
```

7. 위 yaml 로 리소스 생성
```
kubectl create -f customer.yaml
```

8. 연간 스케쥴 페이지에 관한 Service, Pod yaml 확인
```
cat schedule.yaml
```

9. 위 yaml 로 리소스 생성
```
kubectl create -f schedule.yaml
```

10. 로드밸런싱 용 ingress yaml 확인
```
cat lb-ingress.yaml
```

11. 위 yaml 로 리소스 생성
```
kubectl create -f lb-ingress.yaml
```

12. 위에서 생성한 리소스 확인
```
kubectl get po
kubectl get service
kubectl get ingress
```

13. ingress controller 가 만든 Nodeport 유형의 서비스의 Nodeport를 확인
```
kubectl get svc -n ingress-nginx
```

14. 워커노드의 ip와 13과정에서 확인한 Nodeport를 확인하여 접근시도
```
curl <워커노드 ip>:<13에서 확인한 portnumber>
curl <워커노드 ip>:<13에서 확인한 portnumber>/customer
curl <워커노드 ip>:<13에서 확인한 portnumber>/schedule
```

15. clear
```
kubectl delete pod,svc,ingress --all
```
