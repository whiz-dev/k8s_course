# Task 1 - Canary Upgrade

### 애플리케이션의 업데이트 방식중 하나인 카나리 업그레이드 기능 실습
#

1. v1 의 yaml 파일 확인
```
cat v1.yaml
```

2. 위 yaml 로 리소스 생성
```
kubectl create -f v1.yaml
```

3. v1에 대한 ingress yaml 확인
```
cat canary_v1-ingress.yaml
```

4. 위 yaml 로 리소스 생성
```
kubectl create -f canary_v1-ingress.yaml
```
5.

6. hosts 파일 수정
```
echo <masterIP> www.canary.com >> /etc/hosts
cat /etc/hosts
```
