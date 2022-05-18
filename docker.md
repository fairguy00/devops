# Docker 컨테이너

* 컨테이너 생성
    docker create [image]

* 컨테이너 생성 및 시작
    docker run [image]

* 컨테이너 시작

    docker start [container]

* 컨테이너 시작 주요 옵션

    docker run \
    -i \                            # 호스트의 표준 입력을 컨테이너와 연결 (interactive)
    -t \                            # TTY 할당
    --rm \                          # 컨테이너 실행 종료 후 자동 삭제
    -d \                            # 백그라운드 모드로 실행 (detached)
    --name hello-world \            # 컨테이너 이름 지정
    -p 80:80 \                      # 호스트 - 컨테이너 간 포트 바인딩
    -v /opt/example:/example \      # 호스트 - 컨테이너 간 볼륨 바인딩
    fastcampus/hello-world:latest \ # 실행할 이미지
    my-command                      # 컨테이너 내에서 실행할 명령어

* 실행중인 컨테이너 상태 확인

    docker ps

* 전체 컨테이너 상태 확인

    docker ps -a

* 컨테이너 상세 정보 확인

    docker inspect [container]

컨테이너 일시중지 및 재개

    docker pause [container]
    docker unpause [container]

* 컨테이너 삭제 (실행중인 컨테이너 불가)
    docker rm [container]

* 컨테이너 강제 종료 후 삭제 (SIGKILL 시그널 전달)
    docker rm -f [container]

* 컨테이너 실행 종료 후 자동 삭제
    docker run --rm ...

* 중지된 모든 컨테이너 삭제
    docker container prune

* 모든컨테이너 중지후 삭제
    docker rm -f $(docker ps -a -q)

* [IMAGE]를 사용하는 모든 컨테이너를 종료한 후에, 이미지를 삭제
    docker image rm [CONTAINER]

* dangling 필터는 TAG가 없는(none)인 이미지만 필터링 해주므로 -f 옵션으로 dangling=true인 이미지들만 검색한 후 -q 옵션을 통해 이미지의 ID만 가져와서 rmi를 통해 삭제해주는 명령어

    docker rmi $(docker images -f "dangling=true" -q)

*엔트리포인트 (Entrypoint)
도커 컨테이너가 실행할 때 고정적으로 실행되는 스크립트 혹은 명령어
생략할 수 있으며, 생략될 경우 커맨드에 지정된 명령어로 수행

*커맨드 (Command)
도커 컨테이너가 실행할 때 수행할 명령어 혹은 엔트리포인트에 지정된 명령어에 대한 인자 값
[Entrypoint] [Command]
실제 수행되는 컨테이너 명령어

    docker run --entrypoint sh ubuntu:focal
    docker run --entrypoint echo ubuntu:focal hello world

* 실행중인 컨테이너에 명령어를 실행합니다.
    docker exec [container] [command]
* my-nginx 컨테이너에 Bash 셸로 접속하기
    docker exec -i -t my-nginx bash
* my-nginx 컨테이너에 환경변수 확인하기
    docker exec my-nginx env

    docker run -p [HOST IP:PORT]:[CONTAINER PORT] [container]
* nginx 컨테이너의 80번 포트를 호스트 모든 IP의 80번 포트와 연결하여 실행
    docker run -d -p 80:80 nginx

* nginx 컨테이너의 80번 포트를 호스트 127.0.0.1 IP의 80번 포트와 연결하여 실행
    docker run -d -p 127.0.0.1:80:80 nginx
* nginx 컨테이너의 80번 포트를 호스트의 사용 가능한 포트와 연결하여 실행
    docker run -d -p 80 nginx

* expose 옵션은 그저 문서화 용도
    docker run -d --expose 80 nginx
* publish 옵션은 실제 포트를 바인딩
    docker run -d -p 80 nginx

* 호스트의 /opt/html 디렉토리를 Nginx의 웹 루트 디렉토리로 마운트
    docker run -d \
    --name nginx \
    -v /opt/html:/usr/share/nginx/html \
    nginx

## 볼륨 컨테이너
* 특정 컨테이너의 볼륨 마운트를 공유할 수 있습니다.
    docker run -d \
    --name my-volume \
    -it \
    -v /opt/html:/usr/share/nginx/
    html \
    ubuntu:focal
* my-volume 컨테이너의 볼륨을 공유
    docker run -d \
    --name nginx \
    --volumes-from my-volume \
    nginx

## 도커 볼륨
*도커가 제공하는 볼륨 관리 기능을 활용하여 데이터를 보존합니다.   기본적으로 /var/lib/docker/volumes/${volume-name}/_data 에 데이터가 저장됩니다.
* web-volume 도커 볼륨 생성
    docker volume create --name db
* 도커의 web-volume 볼륨을 Nginx의 웹 루트 디렉토리로 마운트
    docker run -d \
    --name fastcampus-mysql \
    -v db:/var/lib/mysql \
    -p 3306:3306 \
    mysql:5.7

### 읽기전용 볼륨 연결
* 볼륨 연결 설정에 :ro 옵션을 통해 읽기 전용 마운트 옵션을 설정할 수 있습니다.
* 도커의 web-volume 볼륨을 Nginx의 웹 루트 디렉토리로 읽기 전용 마운트
    docker run -d \
    --name nginx \
    -v web-volume:/usr/share/nginx/html:ro \
    nginx
* 전체 로그 확인
    docker logs [container]
* 마지막 로그 10줄 확인
    docker logs --tail 10 [container]
* 실시간 로그 스트림 확인
    docker logs -f [container]
* 로그마다 타임스탬프 표시
    docker logs -f -t [container]

* 호스트 운영체제의 로그 저장 경로
    cat /var/lib/docker/containers/${CONTAINER_ID}/${CONTAINER_ID}-json.log

### 로그 용량 제한하기
* 컨테이너 단위로 로그 용량 제한을 할 수 있지만, 도커 엔진에서 기본 설정을 진행할 수도 있습니다. (운영환경에서 필수 설정)
* 한 로그 파일 당 최대 크기를 3Mb로 제한하고, 최대 로그 파일 3개로 로테이팅.
    docker run \
    -d \
    --log-driver=json-file \
    --log-opt max-size=3m \
    --log-opt max-file=5 \
    nginx

## Dockerfile 없이 이미지 생성
* 기존 컨테이너를 기반으로 새 이미지를 생성할 수 있습니다.
* docker commit [OPTIONS] CONTAINER [REPOSITORY[:TAG]]
* ubuntu 컨테이너의 현재 상태를 my_ubuntu:v1 이미지로 생성
    docker commit -a fastcampus -m “First Commit” ubuntu my_ubuntu:v1

## Dockerfile을 기반으로 새 이미지를 생성할 수 있습니다.
    FROM node:12-alpine
    RUN apk add --no-cache python3 g++ make
    WORKDIR /app
    COPY . .
    RUN yarn install --production
    CMD ["node", "src/index.js"]

* docker build [OPTIONS] PATH
* ./ 디렉토리를 빌드 컨텍스트로 my_app:v1 이미지 빌드 (Dockerfile 이용)
    docker build -t my_app:v1 ./
* ./ 디렉토리를 빌드 컨텍스트로 my_app:v1 이미지 빌드 (example/MyDockerfile 이용)
    docker build -t my_app:v1 -f example/MyDockerfile ./

## 빌드 컨텍스트
* 도커 빌드 명령 수행 시 현재 디렉토리(Current Working Directory)를 빌드 컨텍스트(Build Context)라고 합니다.   dockerfile로부터 이미지 빌드에 필요한 정보를 도커 데몬에게 전달하기 위한 목적입니다.
    => [internal] load build definition from Dockerfile 0.0s
    => => transferring dockerfile: 190B 0.0s
    => [internal] load .dockerignore 0.0s
    => => transferring context: 2B 0.0s
    => [internal] load metadata for docker.io/library/node:12-alpine 1.0s
    => [1/5] FROM docker.io/library/node:12-alpine@sha256:0eca266c5fe38ba93aeba 0.0s
    => [internal] load build context 0.1s
    => => transferring context: 4.61MB 0.1s

### .dockerignore
* .gitignore와 동일한 문법을 가지고 있습니다.
* 특정 디렉토리 혹은 파일 목록을 빌드 컨텍스트에서 제외하기 위한 목적입니다.
    # comment
    */temp*
    */*/temp*
    temp?
    *.md
    !README.md