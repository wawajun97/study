# Kubernetes

> 컨테이너를 관리하는 기술

- 컨테이너 오케스트레이션(부하가 생기면 자동으로 인스턴스 추가 -> 동인한 컨테이너를 하나 더 생성하고 로드밸런싱)
  - 중단 없이 버전 업데이트
  - 중앙 제어
    1. 마스터 노드를 두고 관리자는 마스터 노드에 명령
    2. 마스터 노드가 많은 노드들(클러스터)을 관리
  - 상태 관리
  - 서비스 등록 및 조회

## 용어
- 노드 : 가상 서버(=워커노드)
- 마스터노드 : 관리자 역할을 하는 노드
- 노드 + 마스터 노드 = 클러스터
- 포드 : 가장 작은 배포 단위(컨테이너 집합소)
  - 포드는 각자의 ip 주소를 갖는다.
  - 포드는 컨테이너를 여러 개 가질 수 있다.
  - 같은 포드에 컨테이너들은 리소스를 공유하고 서로 소통한다.
  - 포드 내에 네임스페이스를 지정하여 리소스 구분이 가능하다.(예를 들어 한 포드안에 개발과 qa가 같이 있다면 네임스페이스로 리소스를 구분한다)
  - 쿠버네티스의 노드는 여러개의 포드를 가질 수 있고 포드는 여러개의 컨테이너를 가질 수 있다.
  - kubectl get pods
- 리플리카셋 : 여러개의 pod를 관리 -> pod 생성, 삭제
- 디플로이먼트 : 배포 버전 관리 -> 무중단 업데이트
- desired state : 상태체크를 하고 현재 상태가 맞는지 확인
  - 상태 체크 -> 차이점 확인 -> 조치
- api server : 요청을 받고 요청을 실행하는 역할 / 상태를 바꾸거나 조회하는 역할 / etcd와 유일하게 통신
- scheduler : 새로 생성된 포드를 감지하고 실행할 노드를 선택 / 노드의 현재 상태와 포드의 요구사항을 체크
- controller : 상태를 체크하고 원하는 상태 유지 -> 특정 조건만 걸어놓고 검사가 가능하다 / 단일 프로세스로 실행
  - 컨트롤러가 api server에 상태 정보를 요청 -> api server는 권한을 조회 후 etcd에서 상태 정보를 전달해준다.
- etcd : 상태. 데이터를 저장/ 분산 시스템으로 구성 -> 안정성 높임 / key-value / 가볍고 빠르다.
- 프록시 : 네트워크 프록시와 부하 분산 역할
  - 성능상의 이유로 별도의 프록시 프로그램 대신 iptables 또는 ipvs를 사용
- 큐블릿 : 각 노드에서 실행 / 포드를 실행, 중지하고 상태를 체크

## 동작 순서
<img width="1268" height="641" alt="스크린샷 2025-07-26 오후 12 34 11" src="https://github.com/user-attachments/assets/01aa6c84-7521-4f5b-845a-f915c1932534" />

1. api server에 pod 요청
2. pod 요청을 etcd에 메모
3. 컨트롤러가 pod 요청을 알아채고 pod 할당을 요청
4. api server는 etcd에 pod 할당요청으로 수정
5. 스케줄러가 pod 할당요청을 알아채고 pod를 특정 노드에 할달
6. api server는 etcd에 pod 할당/ 미실행으로 수정
7. 큐블릿이 pod 할당/미실행을 알아채고 pod 실행
8. api server는 etcd에 pod 노드 할당 / 실행중으로 수정

> 출처 : https://www.youtube.com/watch?v=SNA1sSNlmy0&list=PLIUCBpK1dpsNf1m-2kiosmfn2nXfljQgb&index=6

## pod 생성 예시

- yaml
```
apiVersion: v1
kind: Pod
metadata:
  name: example
spec:
  containers:
  - name: busybox
    image: busybox:1.25
```

## ReplicaSet 생성 예시

- yaml
```
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: frontend
spec:
  replicas: 3
  selector:
    matchLabels:
      app: frontend
  template:
    metadata:
      labels:
        app: frontend
    spec:
      containers:
      - name:: web
        image: image:v1
```
