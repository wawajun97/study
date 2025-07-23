# Docker

## Docker 개념
도커는 컨테이너와 이미지라는 개념이 있다.
도커란 컨테이너로 패키징하여 실행하는 기술

## Docker vs VM
- Docker : 컨테이너간 커널 공유(하나의 os 위에서 모든 컨테이너가 동작)
- vm : vm마다 각각 os, 커널, 드라이브 등이 분리

  -> 도커가 vm보다 가볍다

## 용어 정리
- 컨테이너는 독립적인 실행 환경
- 이미지는 프로그램을 돌리기 위한 환경들 정의(컨테이너를 실행할 수 있는 실행파일, 설정 값 등을 가지고 있는 것)
  - 이미지를 컨테이너에 올리면 컨테이너에서 이미지에 있는 정보들을 확인 후 하드웨어 할당해서 실행
  - 다른 프로그램에 전혀 영향을 받지 않고 실행 가능

- 이미지는 docker hub라는 곳에 이미 많이 만들어져있다.(tomcat,redis 등)
  - docker run redis 하면 redis 이미지를 다운로드 받아서 redis를 실행한다.
  - 만약 이전에 다운받아서 redis 이미지가 docker cash에 있다면 다운로드 없이 바로 실행한다.

## 명령어
- docker run = docker create + docker start이다.
- docker create = 이미지를 컨테이너에 올려서 컨테이너 생성하는 과정
- docker start = 생성된 컨테이너를 실행

- docker ps로 현재 실행중인 컨테이너를 확인 가능하다.
- docker ps --all로 지금까지 실행했던 컨테이너들을 확인 가능하다.

- docker stop container-id or docker kill container-id로 컨테이너 정지가 가능하다.
  - docker stop을 권장한다. 하지만 어차피 10초동안 stop이 되지 않으면 docker stop을 하더라도 알아서 kill로 멈춘다.

- redis는 redis-cli가 있는데 redis를 도커로 실행시켰을 때 redis-cli 사용법
  - docker exec -it container-id redis-cli로 실행 가능하다.

- 리눅스 환경에서 생성하는 프로세스들은 stdin(standard input), stdout(standard output), stderr가 있다.(얘네들은 통신채널)
  - 정보를 전달할 때 사용한다.
  - 위에 -it = -i -t와 동일하고 -i는 input -t는 포맷을 의미한다 -> 즉, cli에 입력하고 결과값을 출력 가능하도록 해준다.(-i가 없으면 cli에 입력 불가능, -t는 들여쓰기 등 이쁘게 보여줌)

- docker exec -it container-id sh
  - unix명령어를 사용가능하다. 즉, docker 프로그램 내부로 들어가서 내부에서 무언가 작업이 가능하다.(디렉토리 경로보면 리눅스 디렉토리 경로랑 비슷)
  - 디버깅할 때 주로 사용
  - 나갈때는 ctrl + d

- docker run -it image-id sh
  - 실행과 동시에 셸 스크립트로 이동
  - 동일한 이미지를 두개 실행하고 하나의 컨테이너에서 파일을 만들면 나머지 컨테이너에는 파일이 없다. 각자 독립적인 시스템이기 때문

## Docker 파일 예제(커스텀 이미지를 만들기 위하여  Dockerfile이 필요함)
```
#Use an existing docker image as a base
FROM alpine -> 알파인 리눅스라는 리눅스 종류 중 하나를 베이스 이미지로 사용한다는 의미(우분투 등 사용 가능)
#Download and install a dependency
RUN apk add --update redis -> 쉘 명령어 실행(알파인에 redis가 설치됨)
#Tell the image what to do when it starts as a container

```
> CMD ["redis-server"] -> 컨테이너 기본 실행 명령어(컨테이너를 실행하면 redis를 실행해라라는 의미)

## Docker 이미지 생성
- Dockerfile이 있는 디렉토리로 이동
- docker build . 하면 Dockerfile을 기준으로 이미지 생성(docker duild -t 임의의 이미지 이름 . 하면 이미지 이름으로 생성 가능)
- docekr run image-id로 도커 실행 가능(image-id대신 이미지 이름 가능)

## docker compose는 여러개의 컨테이너를 동시에 실행하는 기능뿐만 아니라 컨테이너간의 네트워킹 기능도 제공
- 예를 들어, index.js를 실행하는데 거기서 도커로 실행한 redis에 값을 저장하고 싶으면 각각 실행하면 연결이 안되고 docker compose로 두개를 같이 돌려야 네트워킹이 된다.
  - docker-compose.yml 설정
  - docker-compose up 으로 실행
  - docker-compose up -d : 백그라운드로 실행
  - docker-compose down : docker compose 전체 종료(docker compose로 실행해도 컨테이너 아이디는 각각 생성되지만 이 명령어는 docker compose 내의 모든 컨테이너를 종료한다)
  - docker-compose up --build : 파일에 변경이 있을 때 재시작 시 사용
