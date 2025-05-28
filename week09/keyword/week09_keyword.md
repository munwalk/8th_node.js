- OAuth 2.0
    
     권한 위임(Authorization Delegation)을 위한 **인증 프로토콜**
    
    제3자(클라이언트)가 **사용자의 비밀번호를 직접 알지 않고도**, 사용자의 리소스(데이터)에 접근할 수 있도록 함
    
    **🔹 주요 용어**
    
    | 용어 | 설명 |
    | --- | --- |
    | **Resource Owner** | 리소스의 소유자 (ex. 사용자) |
    | **Client** | 사용자 대신 리소스에 접근하려는 앱 |
    | **Authorization Server** | 인증을 처리하고 토큰을 발급해주는 서버 |
    | **Resource Server** | 보호된 자원이 저장된 서버 (API 서버 등) |
    
    **🔹 흐름 (Authorization Code Grant 기준)**
    
    1. **사용자 → 인증 요청**
        
        클라이언트가 사용자에게 인증 요청 (ex. "카카오 로그인")
        
    2. **사용자 → 인증 서버 로그인 및 동의**
        
        인증 서버(카카오 등)에 로그인 후, 데이터 접근 권한을 부여
        
    3. **Authorization Code 발급**
        
        인증 서버는 클라이언트에게 **authorization code** 전달
        
    4. **Access Token 교환**
        
        클라이언트는 Authorization Code를 이용해 Access Token 발급 요청
        
    5. **리소스 접근**
        
        클라이언트는 Access Token으로 사용자 데이터를 API 요청
        
    
    **🔹 Access Token vs Refresh Token**
    
    | 구분 | 설명 |
    | --- | --- |
    | **Access Token** | API 요청 시 사용되는 토큰, 보통 만료 시간이 짧음 |
    | **Refresh Token** | Access Token 갱신용, 만료 시간 길거나 없음 |
    
    ---
    
- HTTP Cookie
    
    **클라이언트(브라우저)에 저장되는 작은 데이터 조각**
    
    서버가 클라이언트의 상태를 기억하거나 추적할 수 있게 해줌
    
    🔹 **사용 목적**
    
    - 로그인 상태 유지 (세션 관리)
    - 사용자 추적 (트래킹)
    - 사용자 환경 설정 저장
    
    🔹 **구조 예시**
    
    ```
    Set-Cookie: sessionId=abc123; Path=/; HttpOnly; Secure; Max-Age=3600;
    ```
    
    **🔹 주요 속성**
    
    | 속성 | 설명 |
    | --- | --- |
    | `Name=Value` | 쿠키 이름과 값 |
    | `Expires` / `Max-Age` | 쿠키 만료 시각 또는 수명 |
    | `Path` | 어떤 경로에 대해 쿠키를 전송할지 |
    | `Domain` | 어떤 도메인에 대해 쿠키를 전송할지 |
    | `Secure` | HTTPS에서만 전송 |
    | `HttpOnly` | JS에서 접근 불가능 (XSS 방어용) |
    | `SameSite` | 크로스사이트 요청 제한 (CSRF 방어 목적)**Strict / Lax / None** |
    
    **🔹 서버와의 동작 흐름**
    
    1. 서버 → 브라우저 : `Set-Cookie`로 쿠키 설정
    2. 브라우저 → 서버 : 이후 요청마다 `Cookie` 헤더에 쿠키 자동 포함
