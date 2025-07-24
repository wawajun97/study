# Netty
비동기 기반 네트워크 어플리케이션

## Netty 등장
기존 자바 네트워크는 하나의 쓰레드가 i/o작업을 하면 다른 쓰레드는 작업을 못함 -> 블로킹
그리고 한명당 하나의 쓰레드를 사용 -> 사용자가 많아지면 서버 장애 가능성

비동기 이벤트기반 아키텍쳐
이벤트가 발생하면 이벤트 큐에 넣고 이벤트 루프가 큐에서 이벤트를 꺼내 이벤트를 수행하는 동작을 반복
이벤트 루프는 이벤트를 처리하는 단일 쓰레드 -> 블로킹되면 안된다

## Netty 특징

### NIO에 ByteBuffer대신 ByteBuf 사용
  1. 불필요한 복사를 제거 ->소켓의 read write를 Heap 외부에 직접 메모리를 사용 -> 버퍼를 pooling하는 방식 사용
  2. 헤더와 바디가 같이 있어서 복합 버퍼가 가능 -> Unpooled 클래스 사용
  3. 동적 버퍼타입
  4. flip 메소드가 필요없다 -> 읽기 쓰기를 구분하여 포인터(writerIndex, readerIndext)가 있기 때문에 가능
자바 socket 방식은 tcp/udp 개발 방식이 다르다

## Netty 용어
- channel : 사용자 연결
- channelPipeline : 채널과 이벤트 핸들러 사이의 통로
- eventLoop : 여러개의 channel
- eventLoopGroup : 여러개의 eventLoop
- selector : 다수의 channel을 하나의 쓰레드로 관리하는 역할(eventLoop 한개당 하나의 selector를 가짐)
- Bootstrap : netty의 환경설정을 도와주는 클래스
  
## 동작 순서
  1. 사용자 요청 -> 백앤드 코드는 서버 연결 시도
  2. 서버는 클라이언트 연결에 대응하는 소켓 채널 객체 생성
  3. 연결이 완료되면 클라이언트가 데이터 송신
  4. 네티가 소켓 채널에서 읽을 데이터가 있다는 이벤트(데이터)를 채널 파이프라인으로 흘림
  5. 빈 채널 파이프라인 객체 생성 후 채널에 할당 채널에 등록된 ChannelInitializer 인터페이스의 구현체를 가져와 initChannel 메서드 호출
  6. 5에서 등록한 빈 채널 파이프라인을 가져와 커스텀 이벤트 핸들러 객체를 파이프라인에 등록
  7. 채널 파이프라인에 등록된 이벤트 핸들러 중, 인바운드 이벤트 핸들러가 해당하는 메서드 수행
  8. 요청 url에 맞춰 controller, service 로직 실행 service 로직에서 netty manger를 실행
  9. outbound를 돌고 나서 들어온 이벤트에 대해 sendDataProcessing 시작 (*이벤트 루프가 실행)
  10. 단말에서 처리 후 다시 netty에 데이터 전송
  11. 들어온 데이터는 inbound를 돌고 나서 receiveDataProcessing 시작 (*이벤트 루프가 실행)
  12. 해당 프로세서 맞춰 이후 로직 동작
