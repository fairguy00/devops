1. minikube 시스템요구사항

2. core CPU
2GB RAM 필요함

* driver docker 명령어로 클러스터 설치
    minikube start

* 노드정보확인
    kubectl get nodes

* 클러스터 정보확인
    kubectl cluster-info

## minikube 기본사용법

* 쿠버네티스 클러스터 상태 확인
    minikube status

* 쿠버네티스 클러스터 일시중지
    minikube pause

* 쿠버네티스 클러스터 중지
    minikube stop

* 쿠버네티스 클러스터 재개
    minikube unpause

* 쿠버네티스 클러스터 삭제
    minikube delete

* minikube 에드온 목록 확인
    minikube addons list

* minikube 애드온 활성화
    minikube addons enable [addon]

* minikube 애드온 비활성화
    minikube addons disable [addon]

* 쿠버네티스 클러스터 노드에 SSH 접속
    minikube ssh

* 쿠버네티스 클러스터 버전과 대응되는 kubectl 사용
    minikube kubectl ...
