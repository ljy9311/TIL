# docker volume

tmpfs 메모리의 일부 공간을 디스크처럼 사용.    
메모리기 때문에 시스템 종료시키면 날아감.    
메모리가 디스크보다 빠르니까 사용함.

- stateless : 별도의 상태를 저장하지 않는 형태 ex)web
- stareful : 상태를 제공 ex)DB

데이터를 영구적으로 저장하기 위한 목적. 상태를 저장하기 위한 저장소.
1. 바인드 방식

    호스트에 있는 특정 디렉토리 공유. 설정파일이나 소스코드 제공.
    ```
    docker run -d --name web -v /home/vagrant/bindvol:/usr/local/apache2/htdocs httpd -v (호스트 경로):(컨테이너 경로)
    ```
    → /home/vagrant/bindvol와 htdocs가 공유됨

2. 볼륨 방식

    빈 디스크 제공. 어플리케이션이 데이터 생성. ex)MYSQL
    ```
    docker volume create test

    docker run -d --name web -v test:/usr/local/apache2/htdocs httpd
    ```
    → /var/lib/docker/volumes/test/_data 여기 빈 볼륨(test)에 htdocs의 파일들이 저장됨.

***

## 실습

```
$ docker volume create dbvol
$ docker run -d --name db1 -v dbvol:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=1234 mysql:5.7
$ docker exec -it db1 bash
/# mysql -u root -p
mysql> create dabatase abc;
mysql> show database;
+-------------+
| Database    |
+-------------+
| abc         |
| ......      |
+-------------+

$ docker rm -f db1
$ docker run -d --name db2 -v dbvol:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=1234 mysql:5.7
$ docker exec -it db2 bash
/# mysql -u root -p
mysql > show database;
+-------------+
| Database    |
+-------------+
| abc         |    # dbvol1에 저장되어있었음
| ......      |
+-------------+
```