# Netty
비동기 기반 네트워크 어플리케이션

## Netty 등장
- 기존 자바 네트워크는 하나의 쓰레드가 i/o작업을 하면 다른 쓰레드는 작업을 못함 -> 블로킹
- 한명당 하나의 쓰레드를 사용 -> 사용자가 많아지면 서버 장애 가능성

### 비동기 이벤트기반 아키텍쳐
- 이벤트가 발생하면 이벤트 큐에 넣고 이벤트 루프가 큐에서 이벤트를 꺼내 이벤트를 수행하는 동작을 반복
- 이벤트 루프는 이벤트를 처리하는 단일 쓰레드 -> 블로킹되면 안된다

## Netty 특징

### NIO에 ByteBuffer대신 ByteBuf 사용
  1. 불필요한 복사를 제거 -> 소켓의 read write를 Heap 외부에 직접 메모리를 사용 -> 버퍼를 pooling하는 방식 사용
  2. 헤더와 바디가 같이 있어서 복합 버퍼가 가능 -> Unpooled 클래스 사용
  3. 동적 버퍼타입
  4. flip 메소드가 필요없다 -> 읽기 쓰기를 구분하여 포인터(writerIndex, readerIndext)가 있기 때문에 가능
자바 socket 방식은 tcp/udp 개발 방식이 다르다

## Netty 용어
- channel : 사용자 연결
- channelPipeline : channel과 이벤트 핸들러 사이의 통로
- eventLoop : channel의 이벤트를 channelPipeline에 전달(여러 channel 관리 가능)
- eventLoopGroup : 여러개의 eventLoop
- selector : 다수의 channel을 하나의 쓰레드로 관리하는 역할(eventLoop 한개당 하나의 selector를 가짐)
- channelHandler : channel의 이벤트를 처리
  - channelInboundHandler : 사용자가 전송한 데이터를 처리
  - channelOutboundHandler : 사용자에게 전송할 데이터를 처리
- Bootstrap : netty의 환경설정을 도와주는 클래스
- ServerEventGroup : 사용자의 연결을 도와주는 클래스
  - bossGroup(parents) : 사용자의 연결을 도와줌
  - workerGroup(child) : 연결된 channel을 boss에게 받아서 트래픽을 관리


## 동작 순서
  1. 사용자 요청 -> channel 객체 생성하여 연결
  2. 연결이 완료되면 클라이언트가 데이터 송신
  3. 이벤트가 발생하면 selector에게 알려주고 selector가 eventLoop에게 이벤트 전달
  4. eventLoop가 channelPipeline에 이벤트 전달
  5. channelPipeline은 InboundChannelHandler에게 이벤트 전달
  6. 비즈니스 로직 처리

***
reactor netty가 있다는데 찾아봐야함
