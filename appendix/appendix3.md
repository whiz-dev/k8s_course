# Appendix Lab 3. Monitoring tools

# helm 설치

```
curl -sSL https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3 | bash
```

# Repository 확인
```
helm repo add stable https://charts.helm.sh/stable
helm search repo stable
```

# helm bash 환경 설정
```
helm completion bash >> ~/.bash_completion
. /etc/profile.d/bash_completion.sh
. ~/.bash_completion
source <(helm completion bash)
```

# Prometheus, Grafana Repo 추가
```
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo add grafana https://grafana.github.io/helm-charts
```

# 네임스페이스 생성 및 Helm 을 이용 Prometheus & Grafana 모니터링 도구 설치
```
helm install prometheus \
  prometheus-community/kube-prometheus-stack \
  --namespace monitoring \
  --create-namespace
```

# Pod 확인
```
kubectl --namespace monitoring get pods -l "release=prometheus"
```

# 배포된 서비스의 Type 변경 : ClusterIP -> NodePort
```
kubectl edit svc prometheus-kube-prometheus-prometheus -n monitoring
```

# 할당된 노드 포트 확인
```
kubectl get svc prometheus-kube-prometheus-prometheus -n monitoring
```

# 프록시 오픈
```
kubectl port-forward --namespace monitoring svc/prometheus-kube-prometheus-prometheus <위에서 확인한 노드포트>
```

# 브라우저로 프로메테우스 접속
```
http://<workerIP>:<Nodeport>
```

상단 Status -> Target 메뉴
메트릭을 확인할 수 있다.

# 배포된 서비스의 Type 변경 : ClusterIP -> NodePort
```
kubectl edit svc prometheus-grafana -n monitoring
```

# 할당된 노드 포트 확인
```
kubectl get svc prometheus-grafana -n monitoring
```

# 프록시 오픈
```
kubectl port-forward --namespace monitoring svc/prometheus-grafana 9091:80
```

# 브라우저로 그라파나 대시보드 접속
```
http://<workerIP>:<NodePort>
```

# 클러스터 노드에 대한 모니터링 대시보드
좌측 메뉴 -> Import -> 3119 -> load
하단 prometheus 선택 -> Import

# Pod 모니터링 대시보드
좌측 메뉴 -> Import -> 6417 -> load
하단 prometheus 선택 -> Import
