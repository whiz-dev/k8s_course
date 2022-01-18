# Task 2 - Job, cronjob

### job, cronjob 컨트롤러의 동작방식과 기능 확인
#

1. job 조회
```
kubectl get job
```  

3. yaml 확인
```
cat job1.yaml
```

4. job 생성
```
kubectl create -f job1.yaml
```

5. job 확인
```
kubectl get job
kubectl get pod
kubectl describe job job1
```

6. 1분정도 대기 후 재확인
```
kubectl get job
kubectl get pod
kubectl describe job job1
```

7. job1 pod가 실행한 결과 확인
```
kubectl logs <위 과정에서 확인한 pod명>
```

#

8. 두번째 job yaml 확인
```
cat job2.yaml
```

9. job 생성
```
kubectl create -f job2.yaml
```

10. job 확인
```
kubectl get job job2
kubectl get pod
kubectl describe job job2
```

11. 30초 정도 뒤에 재확인
```
kubectl get job job2
kubectl get pod
kubectl describe job job2
```

12. job clear
```
kubectl delete job --all
```

13. cronjob yaml 확인
```
cat cronjob1.yaml
```

14. cronjob 생성
```
kubectl create -f cronjob1.yaml
```

15. 1분정도 뒤에 확인
```
kubectl get cronjob
kubectl get job
kubectl get pod
```

16. 다시 1분정도 뒤에 재확인
```
kubectl get cronjob
kubectl get job
kubectl get pod
```

17. edit 명령을 사용하여 cronjob 중지
```
kubectl edit cronjob cronjob1
```
```
# suspend 값을 false에서 true로 변경합니다.

:%s/false/true/g
:wq
```

18. cronjob 중지된것을 확인
```
kubectl get cronjob
```

19. patch 명령을 사용하여 위 17에서 변경한 true 값을 false로 복구
```
kubectl patch cronjob cronjob1 -p '{"spec" : {"suspend" : false }}'
```

20. 다시 cronjob 을 확인하여 ACTIVE 값이 증가된것을 확인
```
kubectl get cronjob
```

21. clear
```
kubectl delete cronjob --all
```
