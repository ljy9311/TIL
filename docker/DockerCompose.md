# Docker Compose

여러개의 컨테이너를 하나의 파일에 정의.

하나의 서비스를 위해 여러개의 컨테이너를 올려야 하는 경우, 멀티 컨테이너를 하나의 Docker Compose 파일로 정리하고 그 파일을 실행시켜서 컨테이너를 구서알 수 있음.

쿠버네티스를 사용하면 필요없음.

```yaml
# docker-compose.yaml
version: '3'

services:
  web:
    image: httpd:alpine
  db:
    image: mysql:5.7
    environment:
      MYSQL_ROOT_PASSWORD: 1234
```

```
docker-compose.yaml

docker-compose up -d     → detach

docker-compose ps

docker-compose scale web=3  → 사이즈 조정

```
docker ps에서 NAMES = 현재 디렉토리명 + 서비스명 + 인덱스(숫자)

현재 디렉토리에 항상 docker-compose.yaml 파일이 있어야함.     
***

## 설치
https://docs.docker.com/compose/install/