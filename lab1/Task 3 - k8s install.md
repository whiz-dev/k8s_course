# Task 3 - k8s install

###  kubernetes를 설치하고 Master 노드와 Worker 노드를 연동
#
1. kubeadm, kubelet, kubectl 설치

`해당 과정은 Master, Worker1, Worker2 모든 노드에서 진행합니다.`
```
sudo apt-get -y update
sudo apt-get install -y apt-transport-https ca-certificates curl
sudo curl -fsSLo /usr/share/keyrings/kubernetes-archive-keyring.gpg https://packages.cloud.google.com/apt/doc/apt-key.gpg
echo "deb [signed-by=/usr/share/keyrings/kubernetes-archive-keyring.gpg] https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list
```
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
![image](https://user-images.githubusercontent.com/92773629/137877948-678049de-4e17-4e11-be31-00daee62ef62.png)

3. Master 노드 설정
```
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
export KUBECONFIG=/etc/kubernetes/admin.conf
```

4. Calico 네트워크 플러그인 설치
```
kubectl apply -f https://docs.projectcalico.org/manifests/calico.yaml
```
![image](https://user-images.githubusercontent.com/92773629/137878112-476a8d5f-9399-46a9-acaa-5be0a5c0af84.png)

5. kubectl 자동완성 적용
```
source <(kubectl completion bash)
echo "source <(kubectl completion bash)" >> ~/.bashrc
source /etc/bash_completion
alias k=kubectl
complete -F __start_kubectl k
```



6. Worker 노드와 Master 노드 연동

`6번 과정은 Worker1, 2 노드에서만 진행합니다.`
```
<kubeadm join 으로 시작하는 2번과정에서 저장해 두었던 명령어>
```
![image](https://user-images.githubusercontent.com/92773629/138024472-3afc25c2-2de7-4c02-b773-4ec86f4eac8a.png)


7. 연동 확인 (Master노드에서 확인합니다.)
```
kubectl get nodes
```

![image](https://user-images.githubusercontent.com/92773629/138024249-86e26da4-dd00-40ae-a357-d9c776b89443.png)
