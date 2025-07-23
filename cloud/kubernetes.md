# Kubernetes

> 컨테이너를 관리하는 기술

- 오토 스케일링(부하가 생기면 자동으로 인스턴스 추가 -> 동인한 컨테이너를 하나 더 생성하고 로드밸런싱)
- 중단 없이 버전 업데이트

## 가상 서버 이름
- 아마존 -> ec2
- 애저 -> 가상머신
- 구글 클라우드 -> 컴퓨팅 엔진
- 쿠버네티스 -> 노드

## 용어
- 노드 : 가상 서버(=워커노드)
- 마스터노드 : 관리자 역할을 하는 노드
- 노드 + 마스터 노드 = 클러스터

## 클러스터에 이미지 배포
1. 쉘에서 클러스터 연결
2. kubectl create deployment hello-world-rest-api --image=in28min/hello-world-rest-api:0.0.1:REALEASE로 디플로이먼트 생성(이미지는 강사가 도커 허브에 저장해놓은 이미지)
3. kubectl expose deployment hello-world-rest-api --type=LoadBalancer --port=8080로 외부에서 접속 가능하도록 외부에 노출

> kubectl = kubernetes + controller 약자 => 쿠버네티스 명령어

##
kubectl create deployment -> 포드, 리플리카셋, 디플로이먼트 생성
kubectl expose deployment -> 서비스 생성

- 포드 : 작은 전개 유닛 (컨테이너 집합소)
  - 포드는 각자의 ip 주소를 갖는다.
  - 포드는 컨테이너를 여러 개 가질 수 있다.
  - 같은 포드에 컨테이너들은 리소스를 공유하고 서로 소통한다.
  - 포드 내에 네임스페이스를 지정하여 리소스 구분이 가능하다.(예를 들어 한 포드안에 개발과 qa가 같이 있다면 네임스페이스로 리소스를 구분한다)
  - 쿠버네티스의 노드는 여러개의 포드를 가질 수 있고 포드는 여러개의 컨테이너를 가질 수 있다.
  - kubectl get pods

kubectl get replicasets(rs) : 이전에 생성된 리플리카셋 불러오기
리플리카셋 : 특정 수의 포드들이 한번에 실행되도록 함
-> 리플리카셋에서 사용되고 있느 포드가 삭제되면 알아서 포드를 생성하여 다시 실행한다.
-> kubectl scale deployment hello-world-rest-api --replicas=3 => 포드 3개 생성

디플로이먼트 : 버전업할 때 사용
kubectl set image deployment hello-world-res-api(디플로이먼트 이름) hello-world-rest-api(생성할 컨테이너 이름)=in28min/hello-world-rest-api:0.0.2:RELEASE(이미지 경로) => 디플로이먼트에 이미지를 설정하는 것
->디플로이먼트가 작성한 이미지의 리플리카셋을 만들고 리플리카셋은 포드를 만든다.
-> v1 리플리카셋에 포드가 세개였으니 v2 리플리카셋도 포드를 세개 생성한다.
-> 롤링 업데이트 : v1 리플리카셋에서 한개씩 포드를 내리고 v2 리플리카셋에서 한개씩 포드를 올리는 작업

서비스
포드가 생성 삭제되는 것을 클라이언트가 모르게끔 동작시킨다.

desired state : 상태체크를 하고 현재 상태가 맞는지 확인(종류가 굉장히 많음)
상태 체크 -> 차이점 확인 -> 조치

쿠버네티스는 요청에 맞게 들어오면 적어놓고 현재 상태와 비교한다.
마스터 : 체크 및 실행
api server : 요청을 받고 요청을 실행하는 역할 / 상태를 바꾸거나 조회하는 역할 / etcd와 유일하게 통신
scheduler : 새로 생성된 포드를 감지하고 실행할 노드를 선택 / 노드의 현재 상태와 포드의 요구사항을 체크
controller : 상태를 체크하고 원하는 상태 유지 -> 특정 조건만 걸어놓고 검사가 가능하다 / 단일 프로세스로 실행
컨트롤러가 api server에 상태 정보를 요청 -> api server는 권한을 조회 후 etcd에서 상태 정보를 전달해준다.
etcd : 상태. 데이터를 저장/ 분산 시스템으로 구성 -> 안정성 높임 / key-value / 가볍고 빠르다.

노드 : 실제로 컨테이너가 실행
프록시 : 네트워크 프록시와 부하 분산 역할 / 성능상의 이유로 별도의 프록시 프로그램 대신 iptables 또는 ipvs를 사용
큐블릿 : 각 노드에서 실행 / 포드를 실행, 중지하고 상태를 체크

1. api server에 pod 요청
2. pod 요청을 etcd에 메모
3. 컨트롤러가 pod 요청을 알아채고 pod 할당을 요청
4. api server는 etcd에 pod 할당요청으로 수정
5. 스케줄러가 pod 할당요청을 알아채고 pod를 특정 노드에 할달
6. api server는 etcd에 pod 할당/ 미실행으로 수정
7. 큐블릿이 pod 할당/미실행을 알아채고 pod 실행
8. api server는 etcd에 pod 노드 할당 / 실행중으로 수정

pod : 가장 작은 배포 단위 / 고유한 ip를 부여받는다 / pod에는 여러개의 컨테이너가 존재한다.
replicaset : 여러개의 pod를 관리 -> pod 생성, 삭제
deployment : 배포 버전 관리

daemonset : 모든 노드에 pod이 하나씩만 있기를 원할 때 사용
statefulsets : 순서대로 pod을 사용하고 싶을 때
job : 한번 실행하고 죽는 pod을 사용하고 싶을 때

service(clusterip) : 클러스터 내부에서 사용하는 프록시 / pod을 로드밸런싱한다.
service(nodeport) : 노드로 들어온 요청을 받아서 pod을 로드밸런싱
service(loadbalancer) : 여러 노드들을 로드밸런싱(즉 웹에서는 여기에 요청을 하고 로드밸런싱해서 노드를 선택해주고 노드에서는 또 로드밸런싱을 해서 pod을 선택)

ingress : 도메인 또는 경로별 라우팅 -> 서비스는 ip를 기준으로 로드밸런싱을 했다면 ingress는 도메인을 기준으로 서비스를 찾는다.

yaml 파일을 작성하면 api server가 명세를 보고 etcd에 저장 -> 이후에는 위에 로직 탐
