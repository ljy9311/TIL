# 애드온 설치

## 클러스터 외부 서비스
쿠버네티스 클러스터에서 웹의 프론트엔드 서비스를 실행하는 파드의 경우 외부로 노출시켜 접근 가능하도록 구성해야 함.  

## MetalLB
  
MetalLB는 LoadBalancer 타입의 클러스터 외부 서비스를 지원한다.    
LoadBalancer : LoadBalancer + NodePort + ClusterIP   [OSI 레이어 4(TCP/UDP)]    
클라우드 관리형 쿠버네티스 서비스 및 MetalLB를 사용하지 않는 경우, 외부 로드 밸런서가 프로비저닝 되지 않기 때문에 외부 IP(로드밸런서 IP)가 할당되지 않고 \<pending> 상태로 보고함.    
> 설치    

https://metallb.universe.tf/installation/
<!-- .    
.    
.    
.     -->
.    
.    
## Ingress
Ingress Controller는 서비스를 외부에 노출 시키는 방법 중 하나이다.     
HTTP 요청의 주소를 구분해 하나의 인그레스 리소스를 이용해 각 서비스에 라우팅 가능하다. 세션 쿠키 기반의 세션 어피니티를 가지고 있다.  [OSI 레이어 7(HTTP/HTTPS)]     
Nginx HTTP 서버와 리버스 프록시 기능을 통해 제공되고, 노드의 개수만큼 각 노드에 실행된다.
<!-- > 설치    

.    
.    
.    
.     -->
.    
.    

***
## 동적 볼륨 프로비저닝 (Dynamic provisioning)
온프레미스(On-Premise)의 베어메탈 또는 가상머신에 배포하거나, 퍼블릭 클라우드의 관리형 쿠버네티스가 아닌 가상머신에 직접 설치한 경우, Rook 오픈소스를 이용해 쿠버네티스 클러스터 네이티브 스토리지 구현 가능.
## Rook
스토리지를 쿠버네티스에 설치하기 위한 도구    
### Ceph 스토리지
Rook의 Storage Provider.Integrated Storage.
Block(RBD), Object, File(CephFS) 스토리지 다 제공함.

> 설치    

https://rook.io/docs/rook/v1.6/ceph-quickstart.html
- 사전 요구사항(Prerequisites)    
디스크, 파티션, 블록장치 필요  (파일 시스템, 파티션 다 없어야 함)    

[ceph cluster]
````
git clone --single-branch --branch v1.6.7 https://github.com/rook/rook.git

cd rook/cluster/examples/kubernetes/ceph

kubectl create -f crds.yaml -f common.yaml -f operator.yaml

kubectl create -f cluster.yaml (3 worker 이상)
또는
kubectl create -f cluster-test.yaml (1~2 worker)
````
-> 확인 (cluster.yaml로 설치시)    
- rook-ceph-osd * 워커 노드 갯수    
- rook-ceph-mon * 워커 노드 갯수    
- rook-ceph-mgr-a * 1    

````
kubectl -n rook-ceph get pod
````

[block]
````
kubectl create -f csi/rbd/storageclass.yaml
````
[file]
````
kubectl create -f filesystem.yaml (3 worker)
또는
kubectl create -f filesystem-test.yaml (1 worker)

kubectl create -f csi/cephfs/storageclass.yaml
````
[ceph status]    
health: HEALTH_WARN / HEALTH_OK 인지 확인
````
kubectl create -f toolbox.yaml

kubectl -n rook-ceph exec deploy/rook-ceph-tools -- ceph -s
````
-> 스토리지 클래스 확인    
- rook-ceph-block = Ceph RDB 블록 (RWO, ROX)
- rook-cephfs = CephFS 파일 시스템 (RWX, RWO, ROX)

````
kubectl get sc
````