# Docker Command (도커 명령어)

- docker start

- docker stop

- docker pause ↔ unpause

- docker restart

- docker ps = 현재 실행중인 컨테이너 목록   
docker ps -a = 컨테이너 목록 전체

- docker stats = 컨테이너 목록(실시간으로 실행됨)   
docker stats --no-stream 명령어 형태로(한번만)

- docker top 현재 컨테이너 내에 있는 프로세스

- docker run -i = 입력 유지   
docker run -t = allocate a pseudo-TTY    
(psuedo TTY = 표준 입출력을 전송해주는 장치)   
docker run -it = 컨테이너에 접속, bash 실행   
docker run -d = 백그라운드에서 실행

- docker inspect = docker image inspect = 이미지 자세한 보기   
(IMAGE ID가 같으면 같은 이미지)

- docker images = docker image ls = 받은 이미지 리스트

- docker pull (이미지):(태그) = docker image pull = 이미지 받기

- docker search = 이미지 검색

- docker create = 컨테이너 생성   
docker create --name <이름>= 이름지정해서 생성   
(만들때 -it 안붙여서 만들면 start -ai 해도 쉘과 연결안됨)

- docker rm = docker container rm = 컨테이너 삭제   
docker rm -f 실행중인 컨테이너 삭제   
docker rm -f `docker ps -aq` 실행중인 컨테이너 모두 삭제   
docker rmi = docker image rm = 이미지 삭제

- docker container prune 현재 중지중인 컨테이너 모두 삭제

- docker save = 로컬에 있는 이미지를 파일로 저장

- docker logs

- docker diff fe

- docker exec -it fe bash