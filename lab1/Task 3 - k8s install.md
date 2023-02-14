# Task 3 - k8s install

###  kubernetes를 설치하고 Master 노드와 Worker 노드를 연동
#
1. kubeadm, kubelet, kubectl 설치

`해당 과정은 Master, Worker1, Worker2 모든 노드에서 진행합니다.`


필요 패키지 설치
```
sudo apt-get -y update
sudo apt-get install -y apt-transport-https ca-certificates curl
```
레포지토리 추가
```
sudo curl -fsSLo /usr/share/keyrings/kubernetes-archive-keyring.gpg https://packages.cloud.google.com/apt/doc/apt-key.gpg
echo "deb [signed-by=/usr/share/keyrings/kubernetes-archive-keyring.gpg] https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list
```
iptables가 브리지된 트래픽을 보게 하기
```
cat <<EOF | sudo tee /etc/modules-load.d/k8s.conf
br_netfilter
EOF

cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
EOF
sudo sysctl --system
```
kubelet, kubeadm, kubectl 설치
```
sudo apt-get -y update
```
```
sudo apt-get install -y kubelet kubeadm kubectl
```
```
sudo apt-mark hold kubelet kubeadm kubectl
```
```
systemctl enable --now kubelet
```

`2~5 과정은 Master 노드에서만 진행합니다.`

2. kubeadm 초기화
```
kubeadm init --pod-network-cidr=172.16.0.0/16 --apiserver-advertise-address=<Master IP>
```
출력결과(kubeadm join 이하 명령어)를 잘 저장해둡니다.
3~5분 소요됩니다.
![](./img/3-kubeadm-init-result.png)

3. Master 노드 설정
```
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
export KUBECONFIG=/etc/kubernetes/admin.conf
```

4. flannel 네트워크 플러그인 설치
```
kubectl create -f https://github.com/wsjang619/k8s_course/tree/master/lab1/yaml/flannel.yaml
```
![](./img/3-calico-install.png)

5. kubectl 자동완성 적용
```
source <(kubectl completion bash)
echo "source <(kubectl completion bash)" >> ~/.bashrc
source /etc/bash_completion
echo alias k=kubectl >> ~/.bashrc
source ~/.bashrc
complete -F __start_kubectl k
```



6. Worker 노드와 Master 노드 연동

`6번 과정은 Worker1, 2 노드에서만 진행합니다.`
```
<kubeadm join 으로 시작하는 2번과정에서 저장해 두었던 명령어>
```
![](./img/3-kubeadm-join-result.png)


7. 연동 확인 (Master노드에서 확인합니다.)
```
kubectl get nodes
```

![](./img/3-kubectl-get-nodes.png)
