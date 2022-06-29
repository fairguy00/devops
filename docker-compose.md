도커 컴포즈 소개

도커 컴포즈 (Docker Compose)
단일 서버에서 여러 컨테이너를 프로젝트 단위로 묶어서 관리
docker-compose.yml YAML 파일을 통해 명시적 관리
프로젝트 단위로 도커 네트워크와 볼륨 관리
프로젝트 내 서비스 간 의존성 정의 가능
프로젝트 내 서비스 디스커버리 자동화
손 쉬운 컨테이너 수평 확장

프로젝트 / 서비스 / 컨테이너

프로젝트 (Project)
도커 컴포즈에서 다루는 워크스페이스 단위.
함께 관리하는 서비스 컨테이너의 묶음.
프로젝트 단위로 기본 도커 네트워크가 생성 됨.
서비스 (Service)
도커 컴포즈에서 컨테이너를 관리하기 위한 단위.
scale을 통해 서비스 컨테이너의 수 확장 가능.
컨테이너 (Container)
서비스를 통해 컨테이너 관리.

![화면 캡처 2022-06-29 151118](https://user-images.githubusercontent.com/19945223/176365739-99658190-1e60-4cc8-9d26-4b306c1e1374.png)

docker-compose.yml

version, services, networks, volumes 총 4개의 최상위 옵션

버전 (version)
2021년 11월 기준 버전 3.9가 최신
가능한 최신 버전 사용 권장
도커 엔진 및 도커 컴포즈 버전에 따른 호환성 매트릭스 참조할 것
버전 3부터 Docker Swarm과 호환
-> Swarm 서비스를 docker-compose.yml로 정의 가능

![docker-compose](https://user-images.githubusercontent.com/19945223/176366079-8c6f828c-a577-4ae2-b06b-aaa0a2385ff3.png)

docker-compose 명령어: 프로젝트 목록
docker-compose v2 이상에서만 가능

```shell
# 실행중인 프로젝트 목록 확인
$ docker-compose ls
# 전체 프로젝트 목록 확인
$ docker-compose ls -a
```

docker-compose 명령어: 실행 및 종료

```shell
# Foreground로 도커 컴포즈 프로젝트 실행
$ docker-compose up
# Background로 도커 컴포즈 프로젝트 실행
$ docker-compose up -d
# 프로젝트 이름 my-project로 변경하여 도커 컴포즈 프로젝트 실행
$ docker-compose -p my-project up -d
# 프로젝트 내 컨테이너 및 네트워크 종료 및 제거
$ docker-compose down
# 프로젝트 내 컨테이너, 네트워크 및 볼륨 종료 및 제거
$ docker-compose down -v
```

docker-compose 명령어: 서비스 확장

```shell
# web 서비스를 3개로 확장
$ docker-compose up --scale web=3
```

docker-compose 명령어

```shell
#프로젝트 내 서비스 로그 확인
$ docker-compose logs

#프로젝트 내 컨테이너 목록
$ docker-compose ps

프로젝트 내 컨테이너 이벤트 확인
$ docker-compose events

# 프로젝트 내 실행중인 프로세스 목록
$ docker-compose top
#프로젝트 내 이미지 목록
$ docker-compose images
```

주요 사용 목적
로컬 개발 환경 구성
특정 프로젝트의 로컬 개발 환경 구성 목적으로 사용
프로젝트의 의존성(Redis, MySQL, Kafka 등)을 쉽게 띄울 수 있음
자동화된 테스트 환경 구성
CI/CD 파이프라인 중 쉽게 격리된 테스트 환경을 구성하여 테스트 수행 가능
단일 호스트 내 컨테이너를 선언적 관리
단일 서버에서 컨테이너를 관리할 때 YAML 파일을 통해 선언적으로 관리 가능
