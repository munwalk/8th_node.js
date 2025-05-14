- 미들웨어
    - HTTP 요청(Request)과 **응답(Response)** 사이에서 특정 로직을 실행하는 **중간 처리 함수**
    - Express에서 미들웨어는 `req`, `res`, `next`를 인자로 가지며 `app.use()` 혹은 라우트에 직접 연결
    
    ```jsx
    (req, res, next) => {
      // 처리 로직
      next(); // 다음 미들웨어로 넘김
    }
    ```
    
    - 미들웨어 주요 역할
        
        
        | 분류 | 설명 |
        | --- | --- |
        | **요청/응답 가공** | `req`, `res` 객체를 가공하거나 데이터 삽입 (ex. `req.user`) |
        | **인증/인가 처리** | JWT, 세션 등을 통해 사용자 인증/권한 확인 |
        | **에러 처리** | try-catch 대신 전역적으로 오류 응답 처리 |
        | **로깅** | 요청 메서드, 경로, 시간 등을 로그로 기록 |
        | **보안 설정** | `helmet`, `cors` 등으로 보안 관련 HTTP 헤더 설정 |
        | **정적 파일 제공** | 정적 자원 (이미지, JS, CSS 등)을 클라이언트에 서비스 |
    
    ---
    
    - 미들웨어의 종류
    
    | 구분 | 설명 | 예시 |
    | --- | --- | --- |
    | **내장 미들웨어** | Express 기본 제공 | `express.json()`, `express.urlencoded()` |
    | **라우터 미들웨어** | 특정 라우터에만 작동 | `app.use("/api", router)` |
    | **에러 처리 미들웨어** | 4개의 인자 필요 | `app.use((err, req, res, next) => {...})` |
    | **서드파티 미들웨어** | 외부 설치 필요 | `helmet()`, `morgan()`, `cors()` 등 |
    
    ---
    
    - 미들웨어 예시 코드 모음
    
    1. 요청 로그 기록 (morgan 없이 직접 구현)
    
    ```jsx
    app.use((req, res, next) => {
      console.log(`[${new Date().toISOString()}] ${req.method} ${req.url}`);
      next();
    });
    ```
    
    2. JSON 파싱
    
    ```jsx
    app.use(express.json());
    ```
    
    3. URL-encoded 폼 파싱
    
    ```jsx
    app.use(express.urlencoded({ extended: true }));
    ```
    
    4. JWT 인증 미들웨어
    
    ```jsx
    import jwt from 'jsonwebtoken';
    
    const authMiddleware = (req, res, next) => {
      const token = req.headers.authorization?.split(' ')[1];
      if (!token) return res.status(401).json({ message: "토큰이 필요합니다." });
    
      try {
        const user = jwt.verify(token, process.env.JWT_SECRET);
        req.user = user;
        next();
      } catch (err) {
        return res.status(401).json({ message: "유효하지 않은 토큰입니다." });
      }
    };
    ```
    
    5. 전역 에러 핸들러
    
    ```jsx
    app.use((err, req, res, next) => {
      console.error(err.stack);
      res.status(err.status || 500).json({
        success: false,
        message: err.message || "서버 내부 오류가 발생했습니다.",
      });
    });
    ```
    
    6. 정적 파일 제공
    
    ```jsx
    app.use("/static", express.static("public")); // public 폴더의 파일을 /static/ 경로로 제공
    ```
    
    7. 보안 관련 미들웨어
    
    ```jsx
    import helmet from 'helmet';
    import cors from 'cors';
    
    app.use(helmet());  // 보안 헤더 설정
    app.use(cors());    // CORS 허용
    ```
    

---

- HTTP 상태 코드
    - 클라이언트에게 요청 결과를 숫자로 전달하는 **HTTP 표준 규약**
    - 총 5가지 범주:
        
        
        | 범위 | 의미 | 설명 |
        | --- | --- | --- |
        | 1xx | Informational | 처리 중 (거의 사용 안 함) |
        | 2xx | Success | 요청 정상 처리 |
        | 3xx | Redirection | 리다이렉션 필요 |
        | 4xx | Client Error | 클라이언트 요청 오류 |
        | 5xx | Server Error | 서버 오류 |
    
    ---
    
    - 자주 사용하는 상태 코드
    - 2xx: 성공
        
        
        | 코드 | 의미 | 실무 사용 예 |
        | --- | --- | --- |
        | `200` | OK | 일반 요청 성공 (GET 등) |
        | `201` | Created | 리소스 생성 (POST: 회원가입 등) |
        | `204` | No Content | 성공했지만 본문 없음 (DELETE 등) |
    - 4xx: 클라이언트 오류
        
        
        | 코드 | 의미 | 실무 사용 예 |
        | --- | --- | --- |
        | `400` | Bad Request | 유효하지 않은 입력값 |
        | `401` | Unauthorized | 로그인되지 않음 (토큰 없음/만료) |
        | `403` | Forbidden | 권한 없음 (관리자만 접근 등) |
        | `404` | Not Found | 존재하지 않는 자원 |
        | `409` | Conflict | 중복 데이터 (이메일, 닉네임 등) |
    - 5xx: 서버 오류
        
        
        | 코드 | 의미 | 실무 사용 예 |
        | --- | --- | --- |
        | `500` | Internal Server Error | 코드 예외, DB 오류 |
        | `502` | Bad Gateway | 프록시 문제 |
        | `503` | Service Unavailable | 과부하/점검 중 |
    
    ---
    
    - 실무 응답 템플릿 예시
        
        ```jsx
        // 성공 응답
        res.status(200).json({
          success: true,
          data: result,
        });
        
        // 클라이언트 오류
        res.status(400).json({
          success: false,
          message: "필수 입력값이 누락되었습니다.",
        });
        
        // 인증 오류
        res.status(401).json({
          success: false,
          message: "로그인이 필요합니다.",
        });
        
        // 서버 오류
        res.status(500).json({
          success: false,
          message: "서버 오류가 발생했습니다. 관리자에게 문의하세요.",
        });
        ```
        
    
    ---
    
    ### 📌 추가 팁
    
    - 커스텀 에러 객체를 만들어 전역 미들웨어에서 처리하면 유지보수에 좋음
    ```jsx
    class CustomError extends Error {
      constructor(message, status) {
        super(message);
        this.status = status;
      }
    }
    ```
    
    ```jsx
    // 사용 예시
    throw new CustomError("잘못된 요청", 400);
    ```
