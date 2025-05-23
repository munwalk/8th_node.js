- Swagger
    - API를 **정의, 설계, 문서화, 시각화, 테스트**하기 위한 도구 및 프레임워크 모음
    - 현재는 Swagger가 OpenAPI의 하위 도구군처럼 사용되며, OpenAPI 명세를 작성하고 사용하는 데 도움을 줌
    
    🔹 Swagger의 구성요소
    
    | 도구 | 설명 |
    | --- | --- |
    | **Swagger UI** | 작성한 OpenAPI 명세를 웹 UI로 시각화하여 바로 테스트할 수 있는 도구
    개발자와 클라이언트가 직접 API를 클릭해보고 확인 가능 |
    | **Swagger Editor** | YAML 또는 JSON 기반 OpenAPI 문서를 웹에서 작성하고 미리 보기 가능 |
    | **Swagger Codegen** | OpenAPI 명세를 기반으로 클라이언트 SDK, 서버 스텁, 문서를 자동 생성 (Java, Node.js, Python 등 다양한 언어 지원) |
    | **SwaggerHub** | API 협업 도구로, Swagger UI + Editor + 팀 협업 기능 통합
    클라우드에서 API 관리 가능 |
    
    🔹 Swagger 사용 이점
    
    - 문서 자동화로 개발 효율성 증가
    - 팀원 간 API 명세 공유 용이
    - 클라이언트-서버 간 명확한 계약 보장
    - API 테스트 자동화 가능

---

- OpenAPI
    - Swagger에서 발전한 **RESTful API 명세를 위한 표준 포맷**
    - **기계가 이해할 수 있는 API 설명서**를 만들 수 있도록 JSON 또는 YAML 포맷으로 작성됨
    
    🔹 핵심 정보 구조
    
    | 키 | 설명 |
    | --- | --- |
    | **openapi** | 명세 버전 (ex: 3.0.0, 3.1.0) |
    | **info** | API 제목, 설명, 버전, 라이선스 등 |
    | **servers** | 실제 API가 동작하는 서버 주소 목록 |
    | **paths** | 각 API 경로(엔드포인트) 정의 |
    | **components** | 재사용 가능한 정의들 (스키마, 응답, 파라미터 등) |
    | **security** | 인증 정보 (JWT, OAuth 등) |
    | **tags** | API 그룹화(카테고리) |
    | **externalDocs** | 외부 문서 링크 연결 |
    
    🔹 실무에서의 역할
    
    - 백엔드 API를 정의할 때 최초 설계 문서로 사용
    - Swagger, Postman, Redoc 등 다양한 도구와 연동 가능
    - 코드 생성, 테스트 자동화, 모니터링 등과 연결

---

- OpenAPI Component
    - OpenAPI 문서 내 **중복을 피하고 재사용을 높이기 위한 정의 모음**
    - API 명세에서 자주 사용되는 구조를 한 곳에 정의해 두고 `"$ref"`로 참조해서 사용
    
    🔹 주요 Component 종류
    
    | 구성요소 | 설명 | 예시 |
    | --- | --- | --- |
    | **schemas** | 데이터 모델 (DTO처럼) | User, Product 등 객체 정의 |
    | **responses** | 공통 응답 템플릿 | `404 Not Found`, `500 Internal Error` |
    | **parameters** | 공통 쿼리/헤더/경로 파라미터 | `page`, `limit`, `Authorization` |
    | **requestBodies** | POST/PUT 요청 시 Body 정의 | JSON 객체 등 |
    | **securitySchemes** | 인증 방식 정의 | JWT, Basic Auth, OAuth2 |
    | **headers** | 응답 헤더 정의 | `X-RateLimit`, `Set-Cookie` 등 |
    | **examples** | 예시 데이터 정의 | 요청/응답 시 JSON 예제 |
    | **links** | 응답 후 다른 엔드포인트 연결 정의 | `GET /orders/{id}` → `GET /users/{userId}` |
    
    🔹 예시: Components 구조
    
    ```yaml
    components:
      schemas:
        User:
          type: object
          properties:
            id:
              type: integer
            name:
              type: string
            email:
              type: string
    
      parameters:
        PageQuery:
          name: page
          in: query
          required: false
          schema:
            type: integer
            default: 1
    
      responses:
        UnauthorizedError:
          description: 인증 실패
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/ErrorResponse'
    
      securitySchemes:
        bearerAuth:
          type: http
          scheme: bearer
          bearerFormat: JWT
    ```
    

---
