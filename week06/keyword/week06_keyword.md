- ORM
    - 데이터베이스 테이블과 객체(Object)를 매핑(Mapping)해주는 기술
    - SQL 쿼리를 직접 작성하는 대신 객체 지향 프로그래밍 언어로 DB를 조작할 수 있음
    - 주로 데이터베이스 작업(삽입, 조회, 수정, 삭제)을 코드 수준에서 간편하게 처리하기 위해 사용
    
    ---
    
    - **ORM의 장점**
        - SQL 작성 부담 감소 → 코드 일관성 및 생산성 향상
        - 데이터베이스 종속성 완화 (추상화)
        - 객체지향적 코드 구조를 유지 가능
    
    ---
    
    - **ORM의 단점**
        - 복잡한 쿼리 작성에는 오히려 비효율적
        - 성능 튜닝이 어렵거나 제약이 있을 수 있음
        - ORM 추상화가 잘못되면 디버깅 및 최적화가 어려움

---

- Prisma 문서 살펴보기
    - ex. Prisma의 Connection Pool 관리 방법
        - Prisma는 자체적으로 Connection Pool을 관리하지 않고 **DB 드라이버의 Connection Pool**을 그대로 사용함
        - 예를 들어, **PostgreSQL**에서는 `pg` 모듈의 풀링 기능 사용
        - **Connection Pool 수 조정**은 데이터베이스 클라이언트 설정을 통해 진행해야 함
        - **문서 참고**:
            
            Connection Management (Prisma Docs)
            
            https://www.prisma.io/docs/orm/prisma-client/setup-and-configuration/databases-connections/connection-management
            
        - 주요 설정 예시:
            
            ```
            // datasource block에서 connection_limit 설정
            datasource db {
              provider = "postgresql"
              url      = env("DATABASE_URL")
            }
            ```
            
            - Pool 관련 세부 설정은 `DATABASE_URL`에 직접 추가:
                
                ```sql
                postgres://USER:PASSWORD@HOST:PORT/DATABASE?connection_limit=10
                ```
                
        
    
    ---
    
    - ex. Prisma의 Migration 관리 방법
        - Prisma에서는 **Migration**을 코드 기반으로 관리할 수 있음
        - 주요 명령어:
            - `npx prisma migrate dev`: 개발 환경에서 자동으로 Migration 파일 생성 및 적용
            - `npx prisma migrate deploy`: 배포 환경에서 Migration 파일만 적용
            - `npx prisma migrate reset`: DB 초기화 후 스키마 재적용
        - **Migration 폴더 구조**: `/prisma/migrations/` 폴더에 버전별로 생성됨
        - **문서 참고**:
            
            Migrations (Prisma Docs)
            
            https://www.prisma.io/docs/orm/prisma-migrate
            
        - 예시:
            
            ```bash
            npx prisma migrate dev --name add_user_table
            ```
            
            → 새로운 마이그레이션(`add_user_table`)이 생성되고, DB에 반영됨
            
        
    
    ---
    
    - ex. Prisma Client 생성 및 설정 방법
        - Prisma에서는 **Migration**을 코드 기반으로 관리할 수 있음
        - 주요 명령어:
            - `npx prisma migrate dev`: 개발 환경에서 자동으로 Migration 파일 생성 및 적용
            - `npx prisma migrate deploy`: 배포 환경에서 Migration 파일만 적용
            - `npx prisma migrate reset`: DB 초기화 후 스키마 재적용
        - **Migration 폴더 구조**: `/prisma/migrations/` 폴더에 버전별로 생성됨
        - **문서 참고**:
            
            Migrations (Prisma Docs)
            
            https://www.prisma.io/docs/orm/prisma-migrate
            
        - 예시:
            
            ```bash
            npx prisma migrate dev --name add_user_table
            ```
            
            → 새로운 마이그레이션(`add_user_table`)이 생성되고, DB에 반영됨
            
        

---
- ORM(Prisma)을 사용하여 좋은 점과 나쁜 점
    
    
    | 구분 | 내용 |
    | --- | --- |
    | 좋은 점 | 타입 안전성 제공 (TypeScript 연동) / 모델과 DB 간 일관성 유지 / 쿼리 작성이 간편하고 자동완성 지원 / Migration 기능 내장 |
    | 나쁜 점 | 복잡한 쿼리(예: JOIN, WINDOW 함수) 작성이 불편할 수 있음 / Connection Pool 세부 관리 기능 부족 / 일부 기능은 ORM 특성상 성능 저하 우려 존재 / DB 특화 기능(Stored Procedures 등) 사용이 제한적 |

---

- 다양한 ORM 라이브러리 살펴보기
    - Sequelize
        - **특징**
            - Node.js에서 가장 널리 쓰이는 ORM 중 하나
            - PostgreSQL, MySQL, MariaDB, SQLite, MSSQL 등 다양한 DB 지원
            - Promise 기반 비동기 API 제공
        - **장점**
            - 문서와 튜토리얼 자료가 매우 풍부함
            - JOIN, 관계 설정(1:1, 1:N, N:N)이 유연하게 가능
            - 트랜잭션 관리 기능 지원
        - **단점**
            - 타입스크립트(TypeScript) 지원이 약함 → 별도 타입 설정 필요
            - 프로젝트 규모가 커질수록 유지보수가 힘들어질 수 있음
        - **추천 상황**
            - SQL에 익숙하지만 객체지향적으로 코딩하고 싶은 경우
            - 관계가 복잡한 데이터 모델을 다루는 경우
    
    ---
    
    - TypeORM
        - **특징**
            - TypeScript에 최적화된 ORM
            - PostgreSQL, MySQL, MariaDB, SQLite, Oracle, MSSQL, MongoDB 지원
            - Active Record 패턴, Data Mapper 패턴 모두 지원 (코드 스타일 선택 가능)
        - **장점**
            - TypeScript 타입 지원이 매우 강력
            - 데코레이터(@Entity, @Column 등) 기반으로 직관적인 코드 작성 가능
            - 복잡한 관계 설정과 고급 기능(Cascade, Eager/Lazy Loading 등) 지원
        - **단점**
            - 대규모 프로젝트에서 성능 저하나 복잡성 문제가 발생할 수 있음
            - MongoDB 지원은 다소 제한적임
        - **추천 상황**
            - TypeScript 중심의 백엔드 개발을 하고 싶은 경우
            - 복잡한 ORM 기반 로직(트랜잭션, 복합 관계 처리 등)을 구현해야 하는 경우
    
    ---
    
    - Prisma
        - **특징**
            - "차세대 ORM"이라고 불리며 최근 급부상
            - PostgreSQL, MySQL, MariaDB, SQLite, MongoDB 지원 (Mongo 제한적)
            - Prisma Schema로 모델을 정의하고 Prisma Client로 타입 안전 쿼리 작성
        - **장점**
            - 타입 안정성이 매우 강력하고 자동 타입 생성됨
            - CRUD 작업 및 단순한 쿼리 작성이 매우 빠름
            - Prisma 자체 Migration 도구 내장 (Prisma Migrate)
        - **단점**
            - 복잡한 SQL(다중 JOIN, 윈도우 함수 등) 작성이 불편할 수 있음
            - 복잡한 쿼리의 경우 Raw SQL을 직접 작성해야 하는 경우 있음
        - **추천 상황**
            - 간단하고 빠른 CRUD API 서버를 만들고자 할 때
            - 타입 안정성을 가장 중요하게 생각하는 프로젝트
    
    ---
    
    - Objection.js
        - **특징**
            - SQL Query Builder인 Knex.js를 기반으로 한 ORM
            - PostgreSQL, MySQL, SQLite3, MSSQL 지원
        - **장점**
            - Knex.js를 직접 활용할 수 있어 자유도가 매우 높음
            - 복잡한 관계 매핑(Graph Insert, Eager Fetching 등)이 뛰어남
        - **단점**
            - Knex.js에 대한 별도의 이해가 필요함
            - Prisma나 TypeORM만큼 직관적이지 않음
        - **추천 상황**
            - SQL 쿼리를 세밀하게 제어해야 하는 경우
            - 복잡한 비즈니스 로직이 필요한 서버를 개발할 때
    
    ---
    
    - MikroORM
        - **특징**
            - 비교적 최신에 등장한 TypeScript 전용 ORM
            - PostgreSQL, MySQL, MariaDB, SQLite, MongoDB 지원
            - Unit of Work 패턴 사용 (변경사항을 모아서 한 번에 커밋)
        - **장점**
            - 타입 지원이 매우 강력
            - 빠른 속도와 Change Detection(변경 감지) 성능이 우수함
        - **단점**
            - 커뮤니티가 작고, 생태계가 상대적으로 미흡함
            - 대규모 프로젝트에서는 리스크가 있을 수 있음
        - **추천 상황**
            - 성능과 타입 안정성을 둘 다 중요시하는 경우
            - 빠른 개발과 복잡한 ORM 기능이 동시에 필요한 경우
        
    
    ---
    
    ✍️ 한눈에 요약
    
    - **Sequelize** → 전통적인 Node.js ORM, SQL에 익숙한 개발자에게 적합
    - **TypeORM** → TypeScript 기반 대규모 복합 로직 구축 시 적합
    - **Prisma** → 간결하고 빠른 CRUD API + 타입 안정성 최우선
    - **Objection.js** → SQL 쿼리 자유도 극대화 + 복잡한 데이터 모델링
    - **MikroORM** → 최신 기술 스택 + 빠르고 안전한 개발 지향
    
    ---
    
    추가: ORM을 고를 때 고려해야 할 포인트
    
    | 고려사항 | 질문 예시 |
    | --- | --- |
    | 지원하는 데이터베이스 | 사용하려는 DB (PostgreSQL, MySQL 등)를 지원하는가? |
    | 타입스크립트 최적화 여부 | 자동 타입 생성이 필요한가? 타입 안정성이 중요한가? |
    | 복잡한 관계 지원 | 다중 JOIN, 복합 관계, Graph Fetching 기능이 필요한가? |
    | Migration 지원 | 스키마 버전 관리가 필요한가? |
    | 퍼포먼스 | 대량 데이터 처리 성능 이슈는 없는가? |
    | 커뮤니티와 자료량 | 문제가 생겼을 때 레퍼런스나 커뮤니티 질문이 가능한가? |
    | 프로젝트 크기와 성격 | 간단한 CRUD 서비스인가, 복잡한 비즈니스 로직을 다루는가? |

---

- 페이지네이션을 사용하는 다른 API 찾아보기
    - ex. https://docs.github.com/en/rest/using-the-rest-api/using-pagination-in-the-rest-api?apiVersion=2022-11-28
        
        GitHub REST API의 Pagination
        
        - **기본 방식**:
            
            GitHub REST API는 `Link` HTTP 헤더에 **다음(next), 이전(prev)** 페이지의 URL을 담아 반환
            클라이언트는 이 링크를 따라가며 다음 페이지 데이터 요청 가
            
        - **Query 파라미터**:
            - `per_page`: 한 페이지에 포함될 항목 수 (최대 100)
            - `page`: 요청할 페이지 번호 (1부터 시작)
        - **예시**:
            
            ```ruby
            GET https://api.github.com/users/octocat/repos?per_page=10&page=2
            ```
            
        - **응답 헤더 예시 (`Link`)**:
            
            ```
            Link: <https://api.github.com/user/repos?page=3&per_page=10>; rel="next",
                  <https://api.github.com/user/repos?page=50&per_page=10>; rel="last"
            ```
            
        - **장점**:
            - RESTful하고 직관적인 페이지 탐색
            - URL 기반으로 브라우저나 터미널에서 테스트하기 쉬움
        - **단점**:
            - `Link` 헤더를 직접 파싱해야 하므로 클라이언트 로직이 조금 번거로울 수 있음
        - **참고 문서**:
            
            [GitHub REST API - Pagination](https://docs.github.com/en/rest/using-the-rest-api/using-pagination-in-the-rest-api?apiVersion=2022-11-28)
            
        
    
    ---
    
    - ex. https://developers.notion.com/reference/intro#pagination
        
        Notion API의 Pagination
        
        - **기본 방식**:
            
            `start_cursor`와 `has_more` 필드를 기반으로 Cursor 기반 페이지네이션(cursor-based pagination) 사용
            
        - **Query 파라미터**:
            - `page_size`: 페이지당 항목 수 (기본 10, 최대 100)
            - `start_cursor`: 이전 페이지에서 받은 `next_cursor` 값을 넣으면 다음 페이지를 가져옴
        - **응답 예시**:
            
            ```json
            {
              "results": [...],
              "has_more": true,
              "next_cursor": "abc123"
            }
            ```
            
        - **장점**:
            - 삽입/삭제에 강한 커서 기반 방식으로 일관된 페이지 데이터를 제공
            - 순서가 변경되어도 페이지 중복이나 누락 가능성이 적음
        - **단점**:
            - 특정 페이지 번호로 이동하는 것이 어려움 (ex. "5페이지로 바로 이동")
            - 커서를 관리하기 위한 상태 저장 또는 로직 필요
        - **참고 문서**:
            
            [Notion API - Pagination](https://developers.notion.com/reference/intro#pagination)
            
        
    
    ---
    
    - Twitter API v2
        - **방식**: **Cursor 기반 Pagination**
        - **사용 필드**:
            - `pagination_token`: 다음 페이지 요청 시 필요한 커서 토큰
            - `max_results`: 한 페이지당 결과 수 (기본값 있음)
        - **요청 예시**:
            
            ```
            GET https://api.twitter.com/2/users/1234/followers?max_results=100&pagination_token=xyz456
            ```
            
        - **응답 예시**:
            
            ```json
            {
              "data": [...],
              "meta": {
                "next_token": "xyz456",
                "result_count": 100
              }
            }
            ```
            
        - **특징**:
            
            Twitter는 **대용량 데이터**에 적합하도록 **커서 기반으로 설계**, 지속적으로 다음 페이지를 토큰으로 받아 이어서 요청함
            
        - **참고 문서**: [Twitter API - Pagination](https://developer.twitter.com/en/docs/twitter-api/pagination)
        
    
    ---
    
    - Shopify Admin REST API
        - **방식**: **Cursor 기반 Pagination**
        - **사용 필드**:
            - `limit`: 페이지 크기
            - `page_info`: 커서 정보 (`Link` 헤더로 제공됨)
        - **요청 예시**:
            
            ```
            GET /admin/api/2023-01/orders.json?limit=5&page_info=abcdef
            ```
            
        - **응답 헤더**:
            
            ```
            Link: <https://...&page_info=xyz>; rel="next"
            ```
            
        - **특징**:
            
            커머스 대량 데이터를 다루는 만큼 **정확한 커서 기반 페이지네이션** 제공
            `Link` 헤더가 핵심
            
        - **참고 문서**: [Shopify API - Pagination](https://shopify.dev/docs/api/admin-rest/usage/pagination)
