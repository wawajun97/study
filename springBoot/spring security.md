# Spring Security

## 정의
스프링에서 제공하는 인증, 인가 기술

## 주요 필터 및 객체
  1. Authentication : 접근하는 주체의 정보와 권한을 담는 인터페이스
  2. SecurityContext : 로그인한 사용자의 Authentication을 보관
  3. UserDetails : 인증을 성공하면 시큐리티가 생성하는 객체
      - CustomUserDetails를 만들어서 Userdetails 구현
  4. UserDetailsService > loadUserByUserName : 로그인 시도하는 함수
  5. UsernamePasswordAuthenticationFilter > attempAuthentication : 로그인 시도 시 실행
  6. UsernamePasswordAuthenticationFilter > SuccessfulAuthentication : 로그인 성공 시 실행 -> jwt 사용 시 토큰 발행
  7. UsernamePasswordAuthenticationFilter > unSuccessfulAuthentication : 로그인 실패 시 실행
  8. AuthenticationFilter : Authentication 객체를 생성해서 AuthenticationManager에 인증 역할 위임

## Spring Security 인증 순서
AuthenticationFilter -> attempAuthentication -> loadUserByUserName -> SuccessfulAuthentication or unSuccessfulAuthentication 

## JWT 토큰 (json web token)
사용자의 인증 정보를 암호화한 토큰
header, payload, signature로 구성됨
header : signature에서 사용하는 알고리즘 저장
payload : 대상자, 만료 시간 등 정보 저장
signature : header와 payload를 합쳐 암호화한 값

### 로그인 성공 시 절차
사용자 로그인 성공 -> accessToken과 refreshToken 발급 -> api 요청 시마다 클라이언트는 accessToken을 헤더에 넣어서 전달 -> accessToken 검사(만료 시 refreshToken으로 accessToken 재발급) -> 유효한 토큰일 시 api 실행
