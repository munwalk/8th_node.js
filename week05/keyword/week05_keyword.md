- 환경 변수
    
    **운영체제나 실행 환경에 따라 달라지는 값을 외부에서 설정하는 방법**
    
    예: DB 비밀번호, API 키, 포트번호 등 **코드에 직접 적으면 안 되는 값들**
    
    ---
    
    - 환경변수를 쓰는 이유
        
        
        | 이유 | 설명 |
        | --- | --- |
        | **보안** | 중요한 정보(DB 비번, API Key 등)를 코드에 노출하지 않기 위해 |
        | **유연성** | 개발, 테스트, 배포 환경마다 설정을 다르게 하기 위해 |
        | **관리 편의성** | 코드 수정 없이 설정만 바꿔서 동작 제어 가능 |
    
    ---
    
    - 사용 예시 (Node.js 기준)
        
        **`.env` 파일:**
        
        ```
        PORT=3000
        DB_PASSWORD=supersecret
        ```
        
        코드에서 사용:
        
        ```jsx
        import dotenv from 'dotenv';
        dotenv.config();
        
        console.log(process.env.PORT); // 3000
        ```
        
    
    ---
    
    - 환경변수 관리 도구
        
        
        | 환경 | 방법 |
        | --- | --- |
        | Node.js | `dotenv` 패키지 (`npm install dotenv`) |
        | Spring Boot | `application.yml` / `application.properties` |
        | 운영체제 | `export VAR=value` (Linux/macOS), `set VAR=value` (Windows) |
    
    ---
    
    - 주의할 점
        - `.env` 파일은 **절대 Git에 올리면 안 됨** → `.gitignore`에 추가해야 함
        - 필요하면 `.env.example`만 공유해서 형식만 알려주기

---

- CORS
    
    > 다른 출처(origin)의 리소스에 대한 브라우저의 보안 정책
    > 
    > 
    > → 기본적으로 브라우저는 **다른 도메인 간 요청을 제한함** (보안 이유)
    > 
    
    ---
    
    - "출처(origin)"란?
        
        
        | 구성 요소 | 예시 |
        | --- | --- |
        | **도메인** | `localhost`, `example.com` |
        | **포트** | `:3000`, `:8080` |
        | **프로토콜** | `http`, `https` |
        
         이 중 하나라도 다르면 출처가 다르다고 판단됨
        
    
    ---
    
    - CORS가 필요한 상황
        
        클라이언트(localhost:3000)에서 백엔드(`localhost:8080`) API 호출할 때
        
        → **다른 포트** → 출처가 다르므로 **CORS 정책에 걸림**
        
    
    ---
    
    - 해결 방법 (서버에서 설정 필요)
        
        
        | 방법 | 설명 |
        | --- | --- |
        | **Access-Control-Allow-Origin** | 응답 헤더에 이 값이 있어야 브라우저가 허용함 |
        | **OPTIONS 프리플라이트 요청** | `Content-Type: application/json` 등일 경우, 브라우저가 먼저 OPTIONS 요청을 보냄 |
        | **서버에서 CORS 허용 설정** | Node.js: `cors` 미들웨어 사용Spring: `@CrossOrigin` 어노테이션 등 |
    
    ---
    
    - 예시 (Node.js Express 기준)
        
        ```jsx
        import cors from 'cors';
        app.use(cors({
          origin: 'http://localhost:3000', // 허용할 출처
          credentials: true // 쿠키 포함 허용 시
        }));
        ```
        
        ---
        
    
    ✅ 요약
    
    | 항목 | 설명 |
    | --- | --- |
    | **CORS란?** | 다른 출처 간 요청을 제한하는 **브라우저 보안 정책** |
    | **문제되는 이유** | 브라우저는 출처가 다르면 기본적으로 요청 차단 |
    | **해결 방법** | 서버에서 허용할 출처(origin)를 명시적으로 설정 |
    | **클라이언트 설정** | 프론트엔드에서 특별히 할 건 없음 (백엔드 설정 필요) |

---

- DB Connection, DB Connection Pool
    
    **1. DB Connection (단일 연결)**
    
    🔸 개념
    
    > 애플리케이션이 DB와 직접 연결을 생성해서 쿼리를 실행하는 1:1 연결 방식
    > 
    
    🔸 특징
    
    | 항목 | 설명 |
    | --- | --- |
    | **연결 방식** | 요청할 때마다 DB와 새로운 연결 생성 |
    | **해제 방식** | 쿼리 후 연결을 바로 종료 (`close()` 또는 자동) |
    | **속도** | 느림 (연결 생성/종료 비용이 큼) |
    | **리소스 사용** | 요청 수만큼 연결 → CPU/메모리 부담 ↑ |
    | **적합한 상황** | 학습용, 단순 테스트, 요청 수가 적은 경우 |
    | **실무 사용** | ❌ 거의 사용하지 않음 (성능 비효율적) |
    
    🔸 예시 (Node.js)
    
    ```jsx
    const mysql = require('mysql2');
    const conn = mysql.createConnection({
      host: 'localhost',
      user: 'root',
      database: 'umc'
    });
    
    conn.query('SELECT * FROM member', (err, results) => {
      conn.end(); // 요청 후 매번 종료
    });
    
    ```
    
    ---
    
    **2. DB Connection Pool (커넥션 풀)**
    
    🔸 개념
    
    > DB 연결을 미리 여러 개 만들어서 풀(Pool)에 보관하고, 요청이 올 때마다 꺼내 쓰는 방식
    > 
    
    🔸 특징
    
    | 항목 | 설명 |
    | --- | --- |
    | **연결 방식** | 앱 실행 시 **연결 여러 개 생성**해 Pool에 저장 |
    | **해제 방식** | 사용 후 **풀에 반환** (연결은 유지됨) |
    | **속도** | 빠름 (연결 재사용) |
    | **리소스 사용** | 효율적 (정해진 개수만 관리) |
    | **적합한 상황** | 실서비스, 동시 접속 많은 웹앱 |
    | **실무 사용** | 거의 모든 백엔드 프레임워크에서 기본 설정 |
    
    🔸 동작 흐름
    
    1. 서버 시작 시 DB 연결 N개 생성
    2. 클라이언트 요청 → 풀에서 연결 하나 할당
    3. 쿼리 실행
    4. 연결을 닫지 않고 다시 **풀에 반납**
    5. 다음 요청에서 그 연결을 다시 사용
    
    🔸 예시 (Node.js)
    
    ```jsx
    const mysql = require('mysql2/promise');
    
    const pool = mysql.createPool({
      host: 'localhost',
      user: 'root',
      database: 'umc',
      waitForConnections: true,
      connectionLimit: 10 // 최대 10개 연결 유지
    });
    
    // 사용 시
    const conn = await pool.getConnection();
    const [rows] = await conn.query('SELECT * FROM mission');
    conn.release(); // 연결 닫는 대신 반납
    
    ```
    
    ---
    
    ✅ 요약
    
    | 항목 | DB Connection | DB Connection Pool |
    | --- | --- | --- |
    | 정의 | 매 요청마다 새 연결 생성 | 미리 만든 연결을 재사용 |
    | 처리 성능 | 느림 (매번 생성/해제) | 빠름 (재사용) |
    | 리소스 효율 | 낮음 | 높음 |
    | 연결 관리 | 직접 생성/해제 | 풀에서 자동 할당/반납 |
    | 적합한 환경 | 단순 앱, 테스트용 | 실무 환경, 대량 요청 처리 |
    | 실무 사용 여부 | 거의 사용 안 함 | 실무에서 기본 사용 |
    - `DB Connection`은 **원시적이고 느림** – 거의 실무에서는 X
    - `DB Connection Pool`은 **성능과 안정성을 위한 실무 필수 기능**
        
        → 대부분의 프레임워크(Spring, Express, Django 등)에서는 **기본 내장됨**
        

---

- 비동기 (async, await)
    1. **비동기(Asynchronous)**
        
        > 동시에 여러 작업을 처리하는 방식
        > 
        > 
        > 작업이 끝날 때까지 기다리지 않고, 다음 작업을 먼저 실행함
        > 
        
        🔸 예시 (현실에서 비유)
        
        - **동기 처리**: 라면 끓일 때 물 끓을 때까지 계속 기다림
        - **비동기 처리**: 물 끓이는 동안 김치 꺼내고 그릇도 준비함 → **시간 효율 ↑**
        
        ---
        
        - 자바스크립트의 비동기 처리 방식
            
            
            | 방식 | 설명 | 사용법 |
            | --- | --- | --- |
            | **콜백 함수** | 함수 완료 후 호출될 함수를 전달 | `setTimeout(() => {...}, 1000)` |
            | **Promise** | 미래의 완료/실패 결과를 나타내는 객체 | `.then()`, `.catch()` |
            | **async/await** | Promise를 **동기 코드처럼** 작성 가능 | `await fetch(...)` |
        
        ---
        
    2. **async / await**
        
        > Promise 기반의 비동기 코드를 더 깔끔하고 직관적으로 작성하게 해주는 ES8(2017) 문법
        > 
        
        ---
        
        🔹 `async` 키워드
        
        - 함수 앞에 붙이면 **무조건 Promise를 반환하는 비동기 함수**가 됨
            
            ```jsx
            async function sayHello() {
              return 'Hello';
            }
            sayHello().then(console.log); // Hello
            ```
            
        
        🔹 `await` 키워드
        
        - `async` 함수 안에서만 사용 가능
        - `Promise`가 **해결될 때까지 기다림**
        - 마치 동기 코드처럼 작성 가능
        
        ```jsx
        async function fetchUser() {
          const response = await fetch('https://api.com/user');
          const data = await response.json();
          console.log(data);
        }
        ```
        
        ---
        
        - async/await 사용 전 vs 후 비교
            
            ❌ Promise만 사용
            
            ```jsx
            fetch(url)
              .then(res => res.json())
              .then(data => console.log(data))
              .catch(err => console.error(err));
            ```
            
            ✅ async/await로 리팩토링
            
            ```jsx
            async function getData() {
              try {
                const res = await fetch(url);
                const data = await res.json();
                console.log(data);
              } catch (err) {
                console.error(err);
              }
            }
            ```
            
        
        ---
        
        - 장점
            
            
            | 항목 | 설명 |
            | --- | --- |
            | **가독성 향상** | `try/catch`, `await`로 동기 코드처럼 흐름 작성 가능 |
            | **에러 처리 쉬움** | `catch` 대신 `try/catch` 가능 |
            | **체이닝 없이도 가능** | `.then().then()` 구조를 깔끔하게 대체 |
        
        ---
        
        ✅ 요약
        
        | 키워드 | 설명 |
        | --- | --- |
        | `async` | 함수가 **Promise를 반환**하게 만듦 |
        | `await` | Promise가 **해결될 때까지 기다림** |
        | 주요 특징 | 비동기 코드를 **동기처럼 직관적으로** 작성 가능 |
        | 사용 조건 | `await`는 **`async` 함수 안에서만 사용 가능** |
        

---

- try/catch/finally
    
    > 코드 실행 중 발생할 수 있는 에러를 처리하고,
    > 
    > 
    > 에러가 발생하더라도 프로그램이 **안 멈추게** 만드는 구문
    > 
    
    ---
    
    🔹 기본 구조
    
    ```jsx
    try {
      // 에러가 발생할 수 있는 코드
    } catch (error) {
      // 에러가 발생했을 때 실행되는 코드
    } finally {
      // 에러 발생 여부와 상관없이 항상 실행
    }
    ```
    
    ---
    
    - 각 블록의 역할
    
    | 블록 | 설명 | 실행 조건 |
    | --- | --- | --- |
    | `try` | 예외가 발생할 수 있는 **위험한 코드** | 항상 실행 |
    | `catch` | 예외가 발생했을 때 실행됨 | `try` 블록에서 에러가 났을 때만 |
    | `finally` | 에러 발생 여부 상관없이 **무조건 실행** | 항상 실행 (선택적 사용 가능) |
    
    ---
    
    1. 에러 발생 시 처리 예시
    
    ```jsx
    try {
      const result = someUndefinedFunction(); // 에러 발생
      console.log(result);
    } catch (err) {
      console.error('에러 발생:', err.message); // 실행됨
    } finally {
      console.log('항상 실행됨'); // 무조건 실행
    }
    ```
    
    🔸 출력
    
    ```
    에러 발생: someUndefinedFunction is not defined
    항상 실행됨
    ```
    
    ---
    
    2. 에러가 없을 경우 예시
    
    ```jsx
    try {
      const num = 5;
      console.log(num * 2); // 정상 실행
    } catch (err) {
      console.error('에러 발생:', err);
    } finally {
      console.log('항상 실행됨');
    }
    ```
    
    🔸 출력
    
    ```
    10
    항상 실행됨
    ```
    
    ---
    
    - 실무에서의 사용 예시
        - 파일 열기/닫기
        - API 요청 후 리소스 정리
        - DB 연결 후 연결 해제
        - 로딩 상태 해제 (`finally`에서 처리)
            
            ```jsx
            async function loadData() {
              try {
                setLoading(true);
                const res = await fetch('/api/data');
                const data = await res.json();
                console.log(data);
              } catch (err) {
                console.error('데이터 불러오기 실패:', err);
              } finally {
                setLoading(false); // 로딩 상태 해제
              }
            }
            ```
            
    
    ---
    
    ✅ 요약
    
    | 구문 | 역할 |
    | --- | --- |
    | `try` | 에러가 날 가능성 있는 코드를 감쌈 |
    | `catch` | 에러 발생 시 실행할 코드 작성 |
    | `finally` | 에러와 무관하게 항상 실행되는 클린업 코드 (선택사항) |

---
