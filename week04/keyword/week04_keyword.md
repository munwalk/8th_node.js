- ES(ECMA Script)
    - 자바스크립트의 **표준 명세 → 자바스크립트가 어떻게 동작해야 하는지를 정의한 공식 문서**
    - ECMAScript   VS   JavaScript
    
    | 항목 | ECMAScript | JavaScript |
    | --- | --- | --- |
    | **정의** | 자바스크립트의 **표준 명세** | 표준을 기반으로 만든 **프로그래밍 언어** |
    | **포함 범위** | 문법, 타입, 연산자 등 | ECMAScript + 웹 API(DOM, fetch 등) + 브라우저 기능 |
    | **관리 주체** | ECMA International | 브라우저 벤더(Chrome, Firefox 등), TC39 위원회 |
    
    > let, async/await → ECMAScript 문법
    > 
    > 
    > `document.querySelector()` → ECMAScript **아님**, 브라우저 API
    > 
    - ECMAScript가 중요한 이유
        - **버전 기준**으로 자바스크립트의 문법과 기능을 파악할 수 있음
        - **브라우저 호환성 문제**를 이해할 수 있음
        - 최신 기능의 **도입 이유와 역사**를 알 수 있음
        - 코드 작성 시 **표준 기반**으로 설계 가능 (유지보수성 향상)

---

- ES6(ECMAScript 6)
    - 2015년에 도입된 자바스크립트의 6번째 표준안
    - ES6의 주요 변화 및 특징
        
        **1. `let`과 `const`**
        
        - 블록 스코프를 가지는 변수 선언
        - `const`는 재할당 불가 → 불변성 유지에 도움
        
        **2. 화살표 함수 (`=>`)**
        
        - 간결한 함수 표현
        - `this`가 렉시컬(상위 스코프)을 따름
            
            ```jsx
            const add = (a, b) => a + b;
            ```
            
        
        **3. 템플릿 리터럴**
        
        - 백틱(``)을 이용한 문자열 표현
        - `${}`로 변수 삽입 가능
            
            ```jsx
            const name = 'Tom';
            console.log(`Hello, ${name}!`);
            ```
            
        
        **4. 디스트럭처링 할당**
        
        - 객체나 배열의 값을 변수로 간편하게 추출
            
            ```jsx
            const [a, b] = [1, 2];
            const {name, age} = person;
            ```
            
        
        **5. 스프레드/레스트 연산자 (`...`)**
        
        - 배열/객체 복사, 병합, 나머지 추출 등 유용한 문법
            
            ```jsx
            const arr = [1, 2];
            const newArr = [...arr, 3];
            ```
            
        
        **6. 클래스 문법 (`class`)**
        
        - 전통적인 객체지향 스타일 도입
        - 상속 및 메서드 정의 등 명확해짐
            
            ```jsx
            class Animal {
              constructor(name) {
                this.name = name;
              }
              speak() {
                console.log(`${this.name} makes a noise.`);
              }
            }
            ```
            
        
        **7. 모듈 시스템 (`import`, `export`)**
        
        - 코드의 재사용성&구조화 강화
            
            ```jsx
            export const foo = () => {};
            import { foo } from './module.js';
            ```
            
        
        **8. Promise**
        
        - 비동기 처리 더 쉬워짐
            
            ```jsx
            fetch(url)
              .then(response => response.json())
              .then(data => console.log(data));
            ```
            
        
    - ES6를 중요시 하는 이유
        
        1. **현대 자바스크립트의 기반**
        
        - ES6는 이후 버전(ES7~현재)까지의 기반이 되는 핵심적인 변화
        - React, Vue, Node.js 등 모든 주요 생태계에서 기본 전제로 사용
        
        2. **가독성과 유지보수성 향상**
        
        - 코드가 더 간결하고 의도를 잘 드러냄
        - `const`, `let`, arrow function 등으로 에러를 줄이고 유지보수가 쉬워짐
        
        3. **모듈화와 구조화 가능**
        
        - `import/export`로 코드 분리 및 재사용 가능 → 대규모 프로젝트에 필수
        
        4. **비동기 처리 간소화**
        
        - `Promise`, 이후의 `async/await` 도입으로 비동기 로직을 직관적으로 작성 가능
        
        5. **객체지향 프로그래밍(OOP) 친화**
        
        - `class` 문법 도입으로 Java, Python 등 OOP 언어에 익숙한 개발자들이 접근하기 쉬워짐
        
        6. **생산성 향상**
        
        - 스프레드, 디스트럭처링, 템플릿 리터럴 등 편리한 문법으로 반복 작업을 줄이고 효율적인 코딩 가능

---

- **ES Module(ESM)**
    - ECMAScript 2015 (ES6)에서 도입된 **자바스크립트의 공식 모듈 시스템**
        - 이전에는 `CommonJS` (`require`, `module.exports`) 방식이 주로 쓰였지만, ES6 이후에는 **ESM이 표준**으로 자리잡음
    - 주요 특징
        
        
        | 특징 | 설명 |
        | --- | --- |
        | `import` / `export` 문법 | 다른 파일의 기능을 가져오거나(export) 내보낼 수 있음 |
        | **정적 구조** | 컴파일 시점에 import/export가 결정 → 트리 쉐이킹(Tree shaking)에 유리 |
        | **비동기 로딩** | 브라우저 환경에서 모듈은 기본적으로 **비동기**로 로딩됨 |
        | **엄격 모드(Strict mode)** | 자동으로 `"use strict"`가 적용됨 |
        | **스코프 분리** | 각 모듈은 **자체 스코프**를 가짐 → 전역 오염 방지 |
    - 기본 문법
        
        ▸ 모듈 내보내기
        
        ```jsx
        export const foo = () => {};
        import { foo } from './module.js';
        ```
        
        ▸ 모듈 내보내기(`default`)
        
        ```jsx
        // greet.js
        export default function greet(name) {
          console.log(`Hello, ${name}`);
        }
        
        // main.js
        import greet from './greet.js';
        greet('Tom');
        ```
        
        ▸ 모듈 가져오기
        
        ```jsx
        // main.js
        import { add, multiply } from './utils.js';
        console.log(add(2, 3)); // 5
        ```
        
    
    - 사용 환경
        
        
        | 환경 | 설명 |
        | --- | --- |
        | **브라우저** | `<script type="module" src="...">`로 사용 가능 |
        | **Node.js** | `.mjs` 확장자 또는 `"type": "module"` 설정 필요 (`package.json`에) |
        
        ```html
        <!-- HTML에서 모듈 사용 -->
        <script type="module" src="main.js"></script>
        ```
        
        ```json
        // package.json (Node.js에서 ESM 활성화)
        {
          "type": "module"
        }
        ```
        
    
    **✅ ESM이 중요한 이유**
    
    - **표준 모듈 시스템**: 모든 최신 JS 프로젝트(React, Vue, Deno 등)에서 기본 사용
    - **트리 쉐이킹** 가능: 안 쓰는 코드 자동 제거 → 성능 최적화
    - **스코프 분리**로 안정성 향상
    - **브라우저에서도 직접 사용 가능** (번들러 없이)

- ESM vs CommonJS
    
    
    | 항목 | ESM (ES Modules) | CommonJS |
    | --- | --- | --- |
    | 도입 시기 | ES6(2015)에서 도입된 **공식 표준** | Node.js에서 초기에 도입된 **비공식 표준** |
    | 문법 | `import` / `export` | `require()` / `module.exports` |
    | 로딩 방식 | **비동기**, **정적 로딩** | **동기**, **런타임 로딩** |
    | 실행 시점 | **컴파일 시점에 해석됨** | **실행 중에 해석됨** |
    | 스코프 | **모듈 스코프** (전역 오염 X) | 모듈 스코프 (유사) |
    | 트리 쉐이킹 지원 | ✅ 가능 (정적 분석 덕분) | ❌ 불가능 (동적 구조 때문) |
    | 파일 확장자 | `.js` (with `"type": "module"`), `.mjs` | `.js` (기본) |
    | 브라우저 지원 | ✅ 지원 (`<script type="module">`) | ❌ 브라우저 기본 지원 X |
    | Node.js에서 사용 방법 | `"type": "module"` 설정 필요 or `.mjs` 확장자 | 기본적으로 지원 |

---

- 프로젝트 아키텍처
    - 프로젝트 아키텍처가 중요한 이유
        
        
        | 이유 | 설명 |
        | --- | --- |
        | **유지보수 용이성** | 구조가 명확하면 버그 수정과 기능 추가가 쉬움 |
        | **확장성** | 새로운 기능을 독립적으로 추가하거나 변경 가능 |
        | **협업 효율성** | 역할이 분리되어 개발자들이 동시에 작업하기 좋음 |
        | **재사용성 증가** | 반복되는 로직을 분리해 모듈화함 → 코드 재사용성 향상 |
        | **디버깅과 테스트 용이** | 단위 테스트 및 디버깅이 쉬움 (특히 서비스 단위로 나눌 때) |
    - Service-Oriented Architecture(Service Layer Pattern)
        - 비즈니스 로직을 따로 모아놓는 방식
        - 컨트롤러 (사용자의 요청 받음)
            
            서비스 (실제 **비즈니스 처리)**
            
            레포지토리 (DB 작업)
            
        - Controller → Service → Repository (→ DB)
        
        | 구성 요소 | 역할 설명 |
        | --- | --- |
        | **Controller** | HTTP 요청/응답 처리 (REST API 핸들링 등) |
        | **Service** | 핵심 로직 처리 (예: 유저 가입, 결제 처리 등) |
        | **Repository/DAO** | DB 접근 및 쿼리 처리 (ORM 사용 시 repository가 Model 대신 쓰임) |
        
        📌 **장점**
        
        - 책임 분리(Separation of Concerns)
        - 테스트 용이
        - 재사용 가능한 로직 구성 가능
        
    - MVC 패턴(Model-View-Controller)
        - 클라이언트와 UI 중심의 전통적인 웹 아키텍처
        - UI와 데이터를 분리하여 관리
        - 사용자 입력 → Controller → Model → View → 사용자 출력
        
        | 구성 요소 | 역할 설명 |
        | --- | --- |
        | **Model** | 데이터 및 비즈니스 로직 관리 (DB와 직접 연결됨) |
        | **View** | 사용자에게 보여지는 화면 (HTML, 템플릿, 프론트엔드 등) |
        | **Controller** | 사용자의 요청 처리, Model과 View 연결 |
        
        📌 **MVC의 특징**
        
        - UI와 로직 분리
        - 프론트엔드-백엔드 협업이 쉬워짐
        - 빠른 개발에 적합
        
    - 그 외 다른 프로젝트 구조
        - MVVM (Model-View-ViewModel)
            - 주로 **프론트엔드 프레임워크**(Vue, Angular 등)에서 사용
            - View와 ViewModel의 양방향 바인딩
        - Hexagonal Architecture (Ports & Adapters)
            - 도메인 로직을 중심에 두고, 외부 의존성을 어댑터로 격리
            - 입출력, DB, UI 등을 쉽게 바꿀 수 있음 → 테스트에 강함
        - Clean Architecture (Robert C. Martin)
        - 의존성 규칙: **바깥 계층은 안쪽 계층에만 의존**
        - 계층: Entity → Use Case → Interface Adapter → Framework
        - 도메인 로직 중심의 아키텍처
        

---

- 비즈니스 로직
    - 프로그램이 **현실 세계의 비즈니스 규칙을 처리하는 핵심 로직**
        
        > 즉, 단순히 버튼을 클릭하고 화면이 바뀌는 게 아니라,
        > 
        > 
        > "어떤 조건에서 주문이 가능하고",
        > 
        > "회원은 어떤 혜택을 받고",
        > 
        > "포인트는 언제 적립되고",
        > 
        > 이런 **업무/서비스의 규칙**을 코드로 구현한 부분
        > 
        
    - 비즈니스 로직 예시
    
    | 예시 상황 | 비즈니스 로직 내용 |
    | --- | --- |
    | 쇼핑몰에서 결제 | - 재고가 있으면 주문 가능
    - 회원 등급에 따라 할인율 적용
    - 배송비는 5만원 이상 무료 |
    | 도서관리 시스템 | - 연체 시 대여 불가
    - 관리자만 도서 추가 가능
    - 대여는 3권까지 제한 |
    | 키즈카페 예약 앱 | - 평일/주말 시간대별 요금 다름
    - 회원은 쿠폰 사용 가능
    - 인원수 제한 체크 |
    - 비즈니스 로직이 중요한 이유
    
    | 이유 | 설명 |
    | --- | --- |
    | **서비스의 핵심 가치** | 실제 업무 흐름과 사용자의 요구를 반영하는 핵심 부분 |
    | **도메인 전문가와 연결** | 기획자/운영자 등 비개발자가 결정한 규칙을 코드로 정확히 옮기는 다리 역할 |
    | **테스트 중요 영역** | 대부분의 버그와 에러가 이 영역에서 발생함 → 철저한 검증 필요 |
    | **유지보수 대상** | 정책이 바뀌면 가장 먼저 고쳐야 하는 부분 (예: 할인율, 이용 조건 등) |
    - 비즈니스 로직 작성 위치
        - 보통 백엔드 개발에서는 아키텍처에 따라 **비즈니스 로직을 아래와 같이 분리**
            
            
            | 위치 | 역할 설명 |
            | --- | --- |
            | **Controller** | 요청/응답 처리 (로직은 최소화) |
            | **Service Layer** | 💡 **비즈니스 로직을 여기에 집중** |
            | **Repository/Model** | 데이터 접근 (DB 쿼리 처리) |
            
            > ✅ 즉, Service 계층에 로직을 몰아놓고 테스트하거나 재사용할 수 있게 만듦
            > 
            
    - 비즈니스 로직 vs 애플리케이션 로직
        
        
        | 구분 | 설명 |
        | --- | --- |
        | **비즈니스 로직** | 도메인/업무 규칙 처리 (회원 할인, 결제 승인 등) |
        | **애플리케이션 로직** | 앱의 동작 관리 (라우팅, 인증 처리, 로깅, 예외 처리 등) |
    - **좋은 비즈니스 로직 작성 팁**
        - 하나의 함수/메서드는 **하나의 책임**만 가지도록
        - **조건 분기**는 명확하게 → 예외 상황도 잘 처리
        - **서비스 레이어에서 테스트하기 쉽게 설계**
        - **도메인 용어 사용**: `회원`, `포인트`, `할인율` 등 현실 기반 단어로 작성

---

- DTO(Data Transfer Object)
    - 계층 간 데이터 전달을 위한 객체
        - 즉, 클라이언트 → 서버 또는 컨트롤러 → 서비스 → DB로 데이터가 오고 갈 때, **필요한 데이터만 담아서 전달하는 역할**
    - DTO 핵심 목적
        
        
        | 목적 | 설명 |
        | --- | --- |
        | **데이터 전달 전용** | 비즈니스 로직 없이, 오직 **데이터를 담아서 이동**하는 용도 |
        | **보안성 향상** | Entity 전체를 노출하지 않고 필요한 데이터만 전달 가능 |
        | **유지보수 용이** | UI나 API 요구사항에 따라 구조를 유연하게 조절할 수 있음 |
        | **계층 분리** | Controller, Service, Entity 간 **역할 분리**에 도움을 줌 |
    - 회원 가입 요청 DTO예시
        
        1) 클라이언트가 보낸 JSON
        
        ```json
        {
          "username": "jun",
          "email": "jun@example.com",
          "password": "1234"
        }
        ```
        
        2) DTO 클래스 (Java 기준 예시)
        
        ```java
        public class SignupRequestDto {
          private String username;
          private String email;
          private String password;
        }
        ```
        
        3) Controller에서 사용
        
        ```java
        @PostMapping("/signup")
        public ResponseEntity<?> signup(@RequestBody SignupRequestDto requestDto) {
          userService.signup(requestDto);
          return ResponseEntity.ok("회원가입 성공");
        }
        ```
        
        DTO는 **컨트롤러와 서비스 간의 전달 객체**로 쓰이고, 실제 DB의 Entity와는 분리되어 있음
        
    - DTO vs Entity
    
    | 항목 | DTO | Entity |
    | --- | --- | --- |
    | 목적 | 계층 간 **데이터 전달용 객체** | DB와 연결된 **실제 테이블 매핑 객체** |
    | 역할 | 비즈니스 로직 없음 | ORM에 의해 DB CRUD 담당 |
    | 사용 위치 | Controller ↔ Service ↔ Client 간 | Repository ↔ DB 간 |
    | 변경 자유도 | API 변경에 맞춰 자유롭게 조정 가능 | DB 구조 변경과 연관되므로 조심해서 수정해야 함 |
    - DTO 쓰는 이유
        - **Entity를 직접 노출하면 보안/유연성 문제 발생**
            - 예: 비밀번호, 내부 ID 등 클라이언트에게 보내면 안 되는 정보 포함될 수 있음
        - **API 요청/응답을 명확히 문서화/구조화 가능**
        - **다른 계층(프론트, 서비스)에서 필요한 데이터만 선택 가능**
        - **유닛 테스트나 로직 테스트 시 의존성 낮춤**

---
