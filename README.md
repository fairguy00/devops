# devops
$ ssh -i "C:\Users\Hwan\Downloads\fairguy.pem" ubuntu@ec2-3-38-161-20.ap-northeast-2.compute.amazonaws.com

git clone https://github.com/tedilabs/fastcampus-devops

1. minikube 시스템요구사항

2 core CPU
2GB RAM 필요함

driver docker 명령어로 클러스터 설치
\$ minikube start

노드정보확인
\$ kubectl get nodes

클러스터 정보확인
\$ kubectl cluster-info

minikube 기본사용법

쿠버네티스 클러스터 상태 확인
\$ minikube status

쿠버네티스 클러스터 일시중지
\$ minikube pause

쿠버네티스 클러스터 중지
\$ minikube stop

쿠버네티스 클러스터 재개
\$ minikube unpause

쿠버네티스 클러스터 삭제
\$ minikube delete

minikube 에드온 목록 확인
\$ minikube addons list

minikube 애드온 활성화
\$ minikube addons enable [addon]

minikube 애드온 비활성화
\$ minikube addons disable [addon]

쿠버네티스 클러스터 노드에 SSH 접속
\$ minikube ssh

쿠버네티스 클러스터 버전과 대응되는 kubectl 사용
\$ minikube kubectl ...

2. Docker 컨테이너 시작

컨테이너 생성
$ docker create [image]

컨테이너 생성 및 시작

$ docker run [image]

컨테이너 시작

$ docker start [container]

컨테이너 시작 주요 옵션

$ docker run \
-i \                            # 호스트의 표준 입력을 컨테이너와 연결 (interactive)
-t \                            # TTY 할당
--rm \                          # 컨테이너 실행 종료 후 자동 삭제
-d \                            # 백그라운드 모드로 실행 (detached)
--name hello-world \            # 컨테이너 이름 지정
-p 80:80 \                      # 호스트 - 컨테이너 간 포트 바인딩
-v /opt/example:/example \      # 호스트 - 컨테이너 간 볼륨 바인딩
fastcampus/hello-world:latest \ # 실행할 이미지
my-command                      # 컨테이너 내에서 실행할 명령어

실행중인 컨테이너 상태 확인

$ docker ps

전체 컨테이너 상태 확인

$ docker ps -a

컨테이너 상세 정보 확인

$ docker inspect [container]

컨테이너 일시중지 및 재개

$ docker pause [container]
$ docker unpause [container]

컨테이너 삭제 (실행중인 컨테이너 불가)
$ docker rm [container]

컨테이너 강제 종료 후 삭제 (SIGKILL 시그널 전달)
$ docker rm -f [container]

컨테이너 실행 종료 후 자동 삭제
$ docker run --rm ...

중지된 모든 컨테이너 삭제
$ docker container prune

모든컨테이너 중지후 삭제
$ docker rm -f $(docker ps -a -q)

엔트리포인트 (Entrypoint)
도커 컨테이너가 실행할 때 고정적으로 실행되는 스크립트 혹은 명령어
생략할 수 있으며, 생략될 경우 커맨드에 지정된 명령어로 수행

커맨드 (Command)
도커 컨테이너가 실행할 때 수행할 명령어 혹은 엔트리포인트에 지정된 명령어에 대한 인자 값
[Entrypoint] [Command]
실제 수행되는 컨테이너 명령어

$ docker run --entrypoint sh ubuntu:focal
$ docker run --entrypoint echo ubuntu:focal hello world

# 실행중인 컨테이너에 명령어를 실행합니다.
$ docker exec [container] [command]
# my-nginx 컨테이너에 Bash 셸로 접속하기
$ docker exec -i -t my-nginx bash
# my-nginx 컨테이너에 환경변수 확인하기
$ docker exec my-nginx env

$ docker run -p [HOST IP:PORT]:[CONTAINER PORT] [container]
# nginx 컨테이너의 80번 포트를 호스트 모든 IP의 80번 포트와 연결하여 실행
$ docker run -d -p 80:80 nginx
# nginx 컨테이너의 80번 포트를 호스트 127.0.0.1 IP의 80번 포트와 연결하여 실행
$ docker run -d -p 127.0.0.1:80:80 nginx
# nginx 컨테이너의 80번 포트를 호스트의 사용 가능한 포트와 연결하여 실행
$ docker run -d -p 80 nginx

expose 옵션은 그저 문서화 용도
$ docker run -d --expose 80 nginx
# publish 옵션은 실제 포트를 바인딩
$ docker run -d -p 80 nginx

# 호스트의 /opt/html 디렉토리를 Nginx의 웹 루트 디렉토리로 마운트
$ docker run -d \
--name nginx \
-v /opt/html:/usr/share/nginx/html \
nginx

볼륨 컨테이너
특정 컨테이너의 볼륨 마운트를 공유할 수 있습니다.
$ docker run -d \
--name my-volume \
-it \
-v /opt/html:/usr/share/nginx/
html \
ubuntu:focal
# my-volume 컨테이너의 볼륨을 공유
$ docker run -d \
--name nginx \
--volumes-from my-volume \
nginx

도커 볼륨
도커가 제공하는 볼륨 관리 기능을 활용하여 데이터를 보존합니다.
기본적으로 /var/lib/docker/volumes/${volume-name}/_data 에 데이터가 저장됩니다.
# web-volume 도커 볼륨 생성
$ docker volume create --name db
# 도커의 web-volume 볼륨을 Nginx의 웹 루트 디렉토리로 마운트
$ docker run -d \
--name fastcampus-mysql \
-v db:/var/lib/mysql \
-p 3306:3306 \
mysql:5.7

읽기전용 볼륨 연결
볼륨 연결 설정에 :ro 옵션을 통해 읽기 전용 마운트 옵션을 설정할 수 있습니다.
# 도커의 web-volume 볼륨을 Nginx의 웹 루트 디렉토리로 읽기 전용 마운트
$ docker run -d \
--name nginx \
-v web-volume:/usr/share/nginx/html:ro \
nginx