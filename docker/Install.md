# Install

## Ubuntu 환경에서 설치하기   
https://docs.docker.com/engine/install/ubuntu/

```
$ sudo apt-get update

$ sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg \
    lsb-release

$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

$ echo \
  "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

$ sudo apt-get update
$ sudo apt-get install docker-ce docker-ce-cli containerd.io

$ apt-cache madison docker-ce   # 버전 검색
$ sudo apt-get install docker-ce=<VERSION_STRING> docker-ce-cli=<VERSION_STRING> containerd.io

$ sudo docker run hello-world
```

## 접근권한 설정

루트 권한으로 실행하거나 /var/run/docker.sock 파일에 접근권한이 있어야 함.   
이 파일에 지정되어 있는 ip와 포트로 도커 데몬에 접근함.   
접근 권한을 주기 위해서는 사용자를 docker 그룹에 추가해 주면됨.   
```
sudo usermod -aG docker vagrant(사용자)
```
권한이 바뀌면 다시 접속하기.