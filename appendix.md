# 추가 실습

- **Appendix Lab 1. Dashboard**


    # 설치

    ```
    kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.2.0/aio/deploy/recommended.yaml
    ```


    # 토큰 정보 파일들 생성

    ```
    grep 'client-certificate-data' ~/.kube/config | head -n 1 | awk '{print $2}' | base64 -d >> kubecfg.crt
    cat kubecfg.crt

    grep 'client-key-data' ~/.kube/config | head -n 1 | awk '{print $2}' | base64 -d >> kubecfg.key
    cat kubecfg.key

    openssl pkcs12 -export -clcerts -inkey kubecfg.key -in kubecfg.crt -out kubecfg.p12 -name "kubernetes-admin"
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
       namespace: kube-system
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
       name: dashboard-user
       namespace: kube-system
    EOF
    ```  
    # 토큰확인

    ```
    kubectl describe secret $(kubectl -n kube-system get secret | grep dashboard-user | awk '{print $1}') -n kube-system
    ```

    # 토큰값을 메모장에 저장

    # 대시보드 접속 (Chrome)

    ```
    https://<MasterPubicIP>:6443/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/
    ```

    # 안전하지 않음으로 이동 후 위에서 확인한 토큰 입력 로그인

- **Appendix Lab 2. AutoScaling**


    # Metic-server 설치

    ```
    kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/download/v0.5.0/components.yaml
    ```

    # Deployment 편집

    ```
    kubectl edit deployment metrics-server -n kube-system
    ```

    ```
    /kubelet-use-node-status-port

    - --kubelet-use-node-status-port
    - --metric-resolution=15s
    - --kubelet-insecure-tls
    ```
    # 붉은 부분 추가


    # 동작 확인

    ```
    kubectl top node
    ```

    # 실습에 필요한 Deployment와 Service 생성

    ```
    kubectl apply -f [https://k8s.io/examples/application/php-apache.yam](https://k8s.io/examples/application/php-apache.yaml)l
    ```

    # HPA 생성

    ```
    kubectl autoscale deployment php-apache --cpu-percent=50 --min=1 --max=10
    ```

    # 확인

    ```
    kubectl get hpa
    ```

    # 부하 생성(다른 터미널을 추가하여 진행)

    ```
    kubectl run -i --tty load-generator --rm --image=busybox --restart=Never -- /bin/sh -c "while sleep 0.01; do wget -q -O- http://php-apache; done"
    ```

    # 1분정도 뒤 확인

    ```
    kubectl get hpa
    kubectl get deployment php-apache
    ```


    # 부하 중지(부하 생성 시 터미널에서 진행)

    ```
    Ctrl + C
    ```

    # 확인

    ```
    kubectl get hpa
    kubectl get deployment php-apache
    ```

    참고 : [https://kubernetes.io/ko/docs/tasks/run-application/horizontal-pod-autoscale-walkthrough/](https://kubernetes.io/ko/docs/tasks/run-application/horizontal-pod-autoscale-walkthrough/)
