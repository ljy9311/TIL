# kubeadm으로 클러스터 구성하기

## 쿠버네티스 클러스터
쿠버네티스 클러스터는 컨트롤 플레인 노드와 워커 노드로 구성됨.       
***

## 컨트롤 플레인 사용자 정의
control-plane 초기화 (initializing)  
ip 확인 후 진행(여기서는 192.168.200.50)
````
sudo kubeadm init --control-plane-endpoint 192.168.200.50 --pod-network-cidr 192.168.0.0/16 --apiserver-advertise-address 192.168.200.50
````
````
mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config
````
````
curl https://docs.projectcalico.org/manifests/calico.yaml -O

kubectl apply -f calico.yaml
````
***
## 워커 노드정의
컨트롤 플레인에서 \<token>과 \<hash> 확인

````
kubeadm token list
````
````
openssl x509 -pubkey -in /etc/kubernetes/pki/ca.crt | openssl rsa -pubin -outform der 2>/dev/null |\
   openssl dgst -sha256 -hex | sed 's/^.* //'
````
워커 노드에서 컨트롤 플레인으로 kubeadm join
````
kubeadm join --token <token> <control-plane-host>:<control-plane-port> --discovery-token-ca-cert-hash sha256:<hash>
````
컨트롤 플레인에서 클러스터 확인
````
kubectl get nodes
````
****
## 쿠버네티스 버전 업그레이드
> 컨트롤 플레인에서   

업그레이드 할 버전 검색
````
apt update
apt-cache madison kubeadm
````
컨트롤 플레인 노드 업그레이드   
여기서는 1.18.19 → 1.18.20
````
sudo apt-mark unhold kubeadm && \
sudo apt-get update && sudo apt-get install -y kubeadm=1.18.20-00 && \
sudo apt-mark hold kubeadm
````
버전이 잘 받아졌는지 확인
````
kubeadm version
````
kubeadm 버전 업그레이드
````
sudo kubeadm upgrade apply v1.18.20
````
> 워커 노드에서         

kubeadm 버전 업그레이드
````
sudo apt-mark unhold kubeadm && \
sudo apt-get update && sudo apt-get install -y kubeadm=1.18.20-00 && \
sudo apt-mark hold kubeadm
````

> 컨트롤 플레인과 워커 노드에서   

kubelet과 kubectl 업그레이드
```
sudo apt-mark unhold kubelet kubectl && \
sudo apt-get update && sudo apt-get install -y kubelet=1.18.20-00 kubectl=1.18.20-00 && \
sudo apt-mark hold kubelet kubectl
````
kubelet을 다시 시작
````
sudo systemctl daemon-reload
sudo systemctl restart kubelet
````
> 업그레이드 되었는지 확인
````
kubectl get nodes
````
***
