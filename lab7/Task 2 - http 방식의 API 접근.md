# Task 2 - http 방식의 API 접근

### 이번에는 http 방식으로 API 서버에 접근 하는 방식을 학습합니다.
#

1. 프록시 서버를 오픈
```
nohup kubectl proxy --port=8001 --address=<MasterIP> --accept-hosts='^*$' >/dev/null 2>&1 &
```

2. API 접근을 http 방식으로 노드 정보를 확인
```
API 접근을 http 방식으로 노드 정보를 확인
```  
```
curl http://<MasterIP>:8001/api/v1/nodes
```
