# DNS
> 도메인을 ip로 변환하고 접속하는 일련의 과정

- 도메인 : 대상의 ip 주소 등의 정보와 맵핑되는 사람이 알아볼 수 있는 문자열
  - 서브도메인 : 도메인 중 스트링 앞에 추가 문자열이 붙는 도메인 ex) test.example.com
  - APEX 도메인 : 도메인 중 앞에 추가 문자열이 없는 순수한 최상위 도메인 ex) example.com

### 도메인은 계층구조이다

- . 단위로 끊음
ex) test.example.com
  - com : top level domain(TLDs)
  - example : domain
  - test : sub domain
- 계층구조로 분산하여 도메인을 저장하고 검색

### ip 찾는 과정

1. 클라이언트에서 DNS Resolver에게 도메인을 가지고 ip 요청
2. DNS Resolver가 DNS Root에게 클라이언트가 요청한 도메인의 TLDs 서버 정보 요청 
3. DNS Resolver가 TLDs 서버에게 네임 서버 정보 요청
4. DNS Resolver가 네임 서버에게 ip 요청
5. ip를 클라이언트에게 전달
