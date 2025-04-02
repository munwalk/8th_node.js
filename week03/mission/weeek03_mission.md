**홈 화면**

- API Endpoint
    
    GET /api/v1/regions/{regionName}/missions/recommendations
    
    - 조회 기능이므로 GET
    - 사용자가 아직 참여하지 않았고, 현재 선택된 **지역(region)** 내에서 가능한 **새로운 미션 목록**을 보여주는 "추천 미션"으로 의미 명확히 분리
- Path variable
    
    `regionName`: 지역 이름
    
    - 지역별 미션이기 때문에 URL에서 명시적으로 구분해야 함
    - 단 하나의 지역을 선택한 상태에서 조회가 일어나기 때문
- query String
    
    `cursor`: number (선택, 페이징용)
    
    `limit`: number (선택, 기본 10)
    
    - **GET 요청에서 조건을 전달**할 때 사용, 검색/조회 파라미터 전달에 적합
    - 무한스크롤, 페이지네이션 구현 시 필수
- Request Body
    
    X
    
    - `GET` 요청에서는 body 없이 query와 path로만 정보 전달
- Request Header
    
    ```sql
    Authorization: Bearer {accessToken}
    Content-Type: application/json
    ```
    
    - Authorization: 현재 로그인된 사용자 기반으로 "아직 안 한 미션"을 필터링해야 하므로
    - Content-Type: 일반적으로 JSON 전송 포맷 명시

---

**마이 페이지 리뷰 작성**

- API Endpoint
    
    POST /api/v1/reviews
    
    - `POST`: 새로운 리뷰를 생성하므로 생성용 메서드 사용
    - `reviews`는 독립 리소스 → **단수 대상과는 달리 ID 없이 경로 작성**
- Path variable
    
    X
    
    - 특정 `storeId`, `memberId` 등을 Body에서 받으므로 URL에 굳이 명시할 필요 없음
- query String
    
    X
    
    - 생성 작업에 필터 조건은 불필요
- Request Body
    
    ```json
    {
      "memberId": 1,
      "storeId": 10,
      "body": "정말 맛있었어요!",
      "score": 4.5
    }
    ```
    
    - 리뷰 작성에 필요한 모든 정보가 포함됨
    - 보안 또는 민감도는 낮지만 데이터 양이 많고 구조화되어 있으므로 Body 사용이 적절
- Request Header
    
    ```sql
    Authorization: Bearer {accessToken}
    Content-Type: application/json
    ```
    
    - 리뷰 작성은 로그인된 사용자만 가능 → 인증 토큰 필요
    

---

**미션 목록 조회(진행중, 진행 완료)**

- API Endpoint
    
    `GET /api/v1/members/{memberId}/missions`
    
    - 조회만 수행하며 서버 상태를 변경하지 않으므로 `GET` 사용
    - 해당 멤버가 수행한 미션 내역을 조회하므로, 멤버 기준 경로 사용
- Path variable
    
    `memberId`: 사용자 식별 ID
    
    - 어떤 사용자의 미션인지 명확히 특정해야 함 (단 하나의 대상)
- query String
    
    `status`: `"진행중"` 또는 `"진행완료"`
    
    `cursor`: number (선택)
    
    `limit`: number (선택, default 10)
    
    - 조회 조건/필터링을 위해 사용
    - 상태 기반, 페이징 구현 등 **조회 조건은 전부 query로 처리**
- Request Body
X
    - `GET` 방식이므로 조건은 Query로, 데이터는 Body에 포함되지 않음
- Request Header
    
    ```sql
    Authorization: Bearer {accessToken}
    Content-Type: application/json
    ```
    
    - 로그인된 유저가 자신의 미션 목록을 보는 기능이므로 필수
    - `Content-Type`은 실제로 `GET`에서는 영향 없지만 일관성 유지

---

**미션 성공 누르기**

- API Endpoint
    
    PATCH /api/v1/members/{memberId}/missions/{missionId}
    
    - `PATCH`: 전체가 아닌, `status` 하나만 부분 수정 (전체 업데이트는 `PUT`)
    - URL은 대상 명확히 표현 (어떤 멤버가 어떤 미션을 완료했는지)
- Path variable
    
    `memberId`: 회원 ID
    
    `missionId`: 미션 ID
    
    - 상태 변경 대상이 명확하므로 각각 경로에 명시
    - 어떤 사용자의 어떤 미션 상태를 바꿀지 명확하게 표현
- query String
    
    X
    
    - 조건문이 아니라 명확한 대상이므로 Path로 충분함
    - 변경 동작이기 때문에 조회 필터링 필요 없음
- Request Body
    
    ```json
    {
      "status": "진행완료"
    }
    ```
    
    - 수정할 정보(status)만 전달 → `PATCH`의 핵심 목적
    - Body에 민감 정보는 없으나, 명시적 JSON 전달 포맷이므로 Body로 처리
- Request Header
    
    ```sql
    Authorization: Bearer {accessToken}
    Content-Type: application/json
    ```
    
    - 인증된 사용자만 상태 변경 가능 → `Authorization` 필수

---

**회원 가입 하기(소셜 로그인 고려 X)**

- API Endpoint
    
    POST /api/v1/members
    
    - `POST`: 새로운 계정 생성
    - URL은 회원 리소스를 표현하는 `/members` 사용
- Path variable
    
    X
    
    - 생성 대상이 특정 리소스가 아니므로 URL에 ID 등 필요 없음
    - 신규 등록이므로 기존 ID와 무관
- query String
    
    X
    
    - 조건 전달 필요 없음, 모든 정보는 Body에 포함
- Request Body
    
    ```json
    {
      "name": "홍길동",
      "gender": "남성",
      "age": "25",
      "address": "서울시 강남구",
      "specAddress": "역삼동",
      "email": "user@example.com",
      "socialType": "NONE"
    }
    ```
    
    - 사용자 정보는 노출되면 안 되므로 URL이 아닌 Body로 전달
    - JSON 포맷으로 구조화된 데이터 전송
- Request Header
    
    ```sql
    Content-Type: application/json
    ```
    
    - 로그인 전이므로 `Authorization` 필요 없음
    - JSON 데이터 전송 → Content-Type 명시 필수

---

### 기능 명세서 작성의 논리적 단계별 설계 기준

**✅ 1단계: 기능의 주체와 목적을 파악하기**

- "누가", "무엇을 하기 위해" 이 API를 쓰는가?
- 예: 홈 화면 → 사용자가 "자신이 받은 미션"을 조회

✅ 2단계: **HTTP 메서드** 결정

| 목적 | 메서드 | 사용  |
| --- | --- | --- |
| 조회 | `GET` | 목록 조회, 조건 검색 |
| 생성 | `POST` | 회원가입, 리뷰 작성 등 생성 |
| 수정 | `PATCH` | 미션 상태 수정, 정보 일부 변경 |
| 수정 (전체) | `PUT` | 전체 리소스 업데이트, 정보 전체 변경, 설정 초기화 등 |
| 삭제 | `DELETE` |  |

✅ 3단계: **URL 구조 설계 (RESTful하게)**

- 리소스를 명확하게 표현
- `/members/{id}/missions` → "이 멤버의 미션"

📝 기준:

- 엔티티 기준으로 리소스를 표현
- 소유 관계가 명확하면 중첩

✅ 4단계: **Request Body vs Path Variable vs Query String 구분**

| 항목 | 사용 목적 | 예시 | 메서드 |
| --- | --- | --- | --- |
| **Request Body** | 서버에 전달할 실제 데이터 내용 | 닉네임, 리뷰 본문, 점수 등 | GET사용 비추천 |
| **Path Variable** | 특정 리소스를 명확히 지정할 때
URL에서 식별해야 할 리소스 ID 등 | userId, missionId 등 |  |
| **Query String** | 필터링/검색/페이징처럼 **선택적 조건** 전달 → 어떤 "리스트"를 어떻게 "조회"할지를 표현 | status=진행중, cursor=23 등 | GET 多 |

- `body`는 보통 `POST`, `PATCH`에서만 사용
- **`GET`은 body 없이 Path + Query만 씀**

✅ 5단계: 헤더 정보 정의

- `Authorization` 필요 여부 결정
    - 보통 로그인 기반 기능은 `accessToken` 필요
- `Content-Type`도 JSON 전송이면 명시
