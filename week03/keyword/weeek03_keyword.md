### **REST**(**RE**presentational **S**tate **T**ransfer)

- 소프트웨어 아키텍처 스타일 중 하나로, **자원을 이름으로 구분하고 해당 자원의 상태(정보)를 주고받는 통신 방식**
- 네트워크 상에서 Client와 Server 사이의 통신 방식 중 하나이며, HTTP 프로토콜을 기반으로 하여 웹의 장점을 최대한 활용할 수 있다는 장점

---

### **REST 구성요소**

1. 자원 : 소프트웨어가 관리하는 모든 것
    - 각 자원은 **고유한 식별자(ID) 가짐 →** 식별자는 서버에 의해 부여되며, 모든 자원은 서버에 존재
    - ID는 일반적으로 **HTTP URI**로 표현 → 클라이언트는 이 URI를 사용하여 특정 자원을 지정하고 해당 자원에 대한 조작을 요청
        - 학생 정보 자원의 URI가 '/students/student_id'일 때, 클라이언트는 '/students/427'과 같은 URI로 특정 학생의 정보를 요청 가능( '427'은 특정 학생을 식별하는 고유한 ID)
    
    ### **HTTP 프로토콜 메서드**(자원에 대한 행위 나타냄)
 
| 구분 | 업무 | 사용 예시 | 설명 |
| --- | --- | --- | --- |
| **GET** | Read | GET/students/123 | **정보 요청** (지정한 자원의 표현을 가져옴 / 주로 데이터의 조회에 사용) |
| **POST** | Create | POST/students | **새로운 자원을 생성** (클라이언트가 서버에게 새로운 자원을 제출) |
| **PUT** | Update | PUT/students/123 | **지정한 자원의 전체를 업데이트** (클라이언트가 변경된 자원을 전체적으로 업데이트) |
| **PATCH** | Update | PATCH/students/123 | **지정한 자원의 일부를 업데이트** (클라이언트가 변경된 일부 정보만을 업데이트) |
| **DELETE** | Delete | DELETE/students/123 | **지정한 자원을 삭제** (클라이언트가 해당 자원을 삭제 요청) |

2. 표현 : 자원의 상태를 나타내는 데이터
    - 주로 JSON 또는 XML 형식으로 나타나며, 클라이언트와 서버 간의 통신에 사용됨
    - 데이터 교환에서는 표현의 일관성을 유지하는 것이 중요! → 서로 다른 클라이언트가 동일한 자원에 대해 동일한 표현을 받아들일 수 있도록 하는 것이 REST의 핵심 원칙 중 하나
3. URI vs URL
    - URI (Uniform Resource Identifier)
        
        인터넷상의 자원을 식별하기 위한 문자열의 구성으로, URL을 포함하는 개념
        
    - URL (Uniform Resource Locator)
        
        자원의 위치를 의미하며, URI 중 하나로 자원을 찾는 데 사용됨
        

---

### REST 특징

1. 서버-클라이언트 구조
    
    서버와 클라이언트는 각각 역할이 명확히 정의되어 독립적으로 작동
    
    서버 :  API 제공, 비즈니스 로직 처리, 자원 저장
    
    클라이언트 : 사용자 인증, 세션 관리, 유저 인터페이스
    
2. 무상태성(stateless)
    
    각 클라이언트 요청이 서버에 저장된 상태를 가지지 않고, 독립적으로 처리되는 특성
    
    클라이언트가 서버에 대한 이전 상태를 계속 유지할 필요가 없다는 원칙을 기반으로 함
    
3. 캐시처리 기능(cacheable)
    
    REST는 HTTP 프로토콜을 사용하므로 캐싱 기능을 적용 가능
    
    캐시를 통해 한 번 받은 응답을 저장해 두었다가 동일한 요청 시에는 다시 서버에 요청하지 않고 캐시된 응답을 사용 → **중복 요청에 대한 성능을 최적화**하며, 네트워크 대역폭을 절약하고 **응답 시간을 단축**
    
4. 계층 구조(layered system)
    
    RESTful 서비스는 계층 구조로 구성 가능
    
    각 계층은 특정한 역할과 책임을 수행하며, 이는 **단일 책임 원칙**을 강조
    
    → 각 계층은 특정 부분에 집중하므로 전체 시스템을 모듈화 하여 유지보수를 쉽게 해줌
    
5. 인터페이스 일관성(uniform interface)
    
    REST는 균일한 인터페이스를 제공하여 모든 웹 서비스가 **공통의 디자인 원칙**을 따를 수 있도록 함 → 서버와 클라이언트 간의 통신을 단순하고 일관되게 만들어줌
    
6. 자체 표현(self-descriptiveness)
    
    RESTful API는 자체 표현 구조를 가지고 있어 요청 메시지만으로도 쉽게 이해 가능 →  명확한 메타데이터와 함께 클라이언트가 리소스를 효과적으로 활용할 수 있다.
    
    서버는 클라이언트에게 리소스와 관련된 자세한 메타데이터를 제공
    
    → 리소스의 구조, 속성, 허용된 작업 등을 명확하게 설명하며, 클라이언트는 서버의 응답을 받고 빠르게 이해할 수 있게 됨
    

---

### RESTful API

REST 원리를 따르는 시스템은 RESTful이란 용어로 지칭되며, REST API를 제공하는 웹 서비스는 RESTful 하다고 할 수 있음

- API : 컴퓨터 프로그램들이 **서로 효과적으로 상호작용할 수 있도록 데이터와 기능의 집합을 제공하는 도구**

**RESTful API 설계 규칙**

**1. URI에 자원을 명시한다.**

- **목록 조회:** GET /api/products/gadgets
- **개별 자원 조회:** GET /api/animals/wildlife/lions
- **자원 생성:** POST /api/products/gadgets
- **자원 수정:** PUT /api/products/gadgets/{id}
- **자원 삭제:** DELETE /api/products/gadgets/{id}

**2. URI 구분자는 슬래시 '/'를 사용한다.**

**3. URI의 마지막에 '/'를 포함하지 않는다.**

- GET /api/categories/electronics

**4. 하이픈 '-'을 사용한다.**

- GET /api/categories/electronics-products

**5. URI는 소문자로만 작성한다.**

**6. 확장자를 사용하지 않는다.**

- GET /api/news (O)
- GET /api/news.json (X)

**7. Query Parameter로 데이터를 검색한다.**

- GET /api/products?category=electronics

**8. 리소스 간의 관계 표현**

- **관계 표현:** GET /api/users/{userid}/devices
- **복잡한 관계 표현:** GET /api/users/{userid}/preferences/devices

**9. Collection과 Document 표현**

- GET /api/sports/football (컬렉션: sports, 도큐먼트: football)
- GET /api/sports/football/players/10 (컬렉션: sports, players, 도큐먼트: football, 10)

---

### **REST API vs** RESTful API

RESTful API는 **REST의 설계 규칙을 잘 따르는 API**를 의미하며, 주소만으로도 요청의 내용을 이해하기 쉽고 사용하기 쉽도록 설계됨

**REST API**

1. 단순히 HTTP 요청을 통해 웹 서비스에 액세스 할 수 있도록 하는 인터페이스를 의미

2. HTTP 프로토콜을 기반으로 하며, 주로 CRUD 작업을 수행하기 위한 URI 엔드포인트를 제공

3. MVC(Model View Controller) 패턴을 따르지 않아도 되며, 특정한 구현 방식에 제약을 받지 않음

**RESTful API**

1. REST의 설계 규칙을 따르는 API를 의미하며, 주소만으로도 요청의 내용을 쉽게 이해하고 사용할 수 있도록 설계됨

2. HTTP 프로토콜과 해당 시맨틱을 사용하여 사람이 사용하기 쉬운 인터페이스를 만드는 아키텍처 스타일

3. URI 엔드포인트의 구조, HTTP 메서드의 사용, 표현 형식 등을 통해 REST의 원칙을 적용하여 일관성 있게 디자인

---

### Request Header (클라이언트 → 서버로 보내는 헤더)

- **내가 요청 보낼 때 같이 담는 정보**

요청 헤더로 자주 쓰는 것들

| 헤더 | 역할 | 예시 |
| --- | --- | --- |
| **Authorization** | 사용자 인증 정보 전달 | `Authorization: Bearer {JWT}` |
| **Content-Type** | 내가 보낸 데이터 형식 알려줌 | `Content-Type: application/json` |
| **Accept** | 서버 응답 형식 지정 | `Accept: application/json` |
| **User-Agent** | 클라이언트 정보 (브라우저, 앱 등) | `User-Agent: Mozilla/5.0` |
| **Origin** | 요청의 출처 (CORS에서 중요) | `Origin: https://myapp.com` |
| **Referer** | 이전 페이지 주소 | `Referer: https://myapp.com/home` |
| **Cookie** | 세션 등 저장된 값 전송 | `Cookie: sessionId=abc123` |
| **Cache-Control** | 요청에 대해 캐시 무시 등 지정 | `Cache-Control: no-cache` |
| **If-Modified-Since** | 조건부 요청용 (수정된 이후면 응답 받기) | `If-Modified-Since: ...` |

---

### 명세서

- **API Endpoint** → 해당 API를 호출하기 위한 HTTP 메소드, URL을 포함
- **Path variable  →** 단 하나의 특정 대상을 지목
- **query String →** GET 요청에서 전달 할 정보가 여러 개일 경우(보통 검색/조회 때 사용)
- **Request Body**
    
    **→** POST, PUT, PATCH 요청 시 전달되는 JSON 데이터
    
    → 보안이 필요한 민감 정보 또는 구조화된 데이터는 Body에 담아 전달
    
- **Request Header →** 전송에 관련된 기타 정보들이 담기는 부분

---

| 구분 | 설명 | 실무 예 |
| --- | --- | --- |
| Path Variable | URL에 포함되는 변수 | `/members/3` |
| Query String | URL 뒤에 조건 | `/missions?status=progress` |
| Request Body | 실제 보낼 내용 | `{ "body": "맛있어요" }` |
| **Request Header** | 부가 정보 | `Authorization`, `Content-Type` 등 |

[https://isaac-christian.tistory.com/entry/Spring-REST-REST-API-RESTful-API의-특징#📌REST의 구성 요소-1](https://isaac-christian.tistory.com/entry/Spring-REST-REST-API-RESTful-API%EC%9D%98-%ED%8A%B9%EC%A7%95#%F0%9F%93%8CREST%EC%9D%98%20%EA%B5%AC%EC%84%B1%20%EC%9A%94%EC%86%8C-1)
