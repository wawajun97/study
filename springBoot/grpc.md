# GRPC
> 구글에서 개발한 고성능 오픈 소스 원격 프로시저 호출(RPC) 프레임워크

> RPC : RPC는 서로 다른 주소 공간에서 실행되는 프로세스 간의 통신을 가능하게 한다 -> 내부 서버 코드인 것처럼 작동

## GRPC 특징
- 언어 호환성이 좋다
- HTTP/2 기반이라 성능이 좋다
- 바이너리 기반이라 효율적이다

## 구현

1. .proto 파일을 생성하여 서비스 구현 (/src/main/proto 경로 아래에 생성)
   > 메소드와 메시지 타입을 정의

    - .proto 파일
   ```
    syntax = "proto3";

    option java_multiple_files = true;
    option java_package = "com.example.grpc_ex";
    service SpeechService {
      rpc recognize (stream SpeechRequest) returns (stream SpeechResponse);
    }
    
    message SpeechRequest {
      bytes data = 1;
    }
    
    message SpeechResponse {
      string text = 1;
    }
   ```

- 패키지명을 제대로 설정 안하면 다른 클래스에서 인식 못 할 가능성 있음

2. build.gradle 설정

  ```
  plugins {
	id 'com.google.protobuf' version '0.9.4'
  }

  dependencies {
      implementation 'javax.annotation:javax.annotation-api:1.3.2'
      implementation 'com.google.protobuf:protobuf-java:3.24.0'
      implementation 'io.grpc:grpc-protobuf:1.58.0'
      implementation 'io.grpc:grpc-stub:1.58.0'
  }

  protobuf {
      protoc {
        artifact = "com.google.protobuf:protoc:3.24.0"
      }
      plugins {
        grpc {
          artifact = "io.grpc:protoc-gen-grpc-java:1.58.0"
        }
      }
      generateProtoTasks {
        all().each { task ->
          task.plugins {
            grpc {}
          }
        }
      }
    }
  ```


3. 빌드 실행 시 자바 파일 자동 생성 (build/generated/source/proto/main 아래에 생성)

<img width="278" height="242" alt="스크린샷 2025-08-04 오후 8 01 57" src="https://github.com/user-attachments/assets/acaa4ded-7a70-482a-bf9e-64e8ed2fcb88" />
