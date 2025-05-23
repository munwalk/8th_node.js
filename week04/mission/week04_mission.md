![Image](https://github.com/user-attachments/assets/727dd2b6-5eef-4d5e-9001-208807652209)

```
-- 외래키 제약 끄기 (테이블 삭제 시 외래키 오류 방지)
SET FOREIGN_KEY_CHECKS = 0;

-- 기존 테이블 삭제 (존재하는 경우만 삭제됨)
DROP TABLE IF EXISTS member_agree;
DROP TABLE IF EXISTS review_image;
DROP TABLE IF EXISTS review;
DROP TABLE IF EXISTS member_mission;
DROP TABLE IF EXISTS mission;
DROP TABLE IF EXISTS store;
DROP TABLE IF EXISTS region;
DROP TABLE IF EXISTS member_prefer;
DROP TABLE IF EXISTS food_category;
DROP TABLE IF EXISTS member;
DROP TABLE IF EXISTS terms;

-- member: 사용자 기본 정보 테이블
CREATE TABLE member (
    id BIGINT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(20),
    gender VARCHAR(10),
    age INT,
    address VARCHAR(40),
    spec_address VARCHAR(40),
    phone_num VARCHAR(13),
    status VARCHAR(15),
    inactive_date DATETIME(6),
    social_type VARCHAR(10),
    created_at DATETIME(6),
    updated_at DATETIME(6),
    email VARCHAR(50),
    point INT
);

-- region: 지역 정보 테이블
CREATE TABLE region (
    id BIGINT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(20),
    created_at DATETIME(6),
    updated_at DATETIME(6)
);

-- store: 가게 정보, 지역 테이블 참조
CREATE TABLE store (
    id BIGINT PRIMARY KEY AUTO_INCREMENT,
    region_id BIGINT,
    name VARCHAR(50),
    address VARCHAR(50),
    score FLOAT,
    created_at DATETIME(6),
    updated_at DATETIME(6),
    FOREIGN KEY (region_id) REFERENCES region(id)
);

-- terms: 이용 약관 테이블
CREATE TABLE terms (
    id BIGINT PRIMARY KEY AUTO_INCREMENT,
    title VARCHAR(20),
    body TEXT,
    optional BOOLEAN,
    created_at DATETIME(6),
    updated_at DATETIME(6)
);

-- food_category: 음식 카테고리 테이블
CREATE TABLE food_category (
    id BIGINT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(15),
    Column4 VARCHAR(15)
);

-- member_prefer: 회원의 음식 카테고리 선호 정보
CREATE TABLE member_prefer (
    id BIGINT PRIMARY KEY AUTO_INCREMENT,
    member_id BIGINT,
    category_id BIGINT,
    created_at DATETIME(6),
    updated_at DATETIME(6),
    FOREIGN KEY (member_id) REFERENCES member(id),
    FOREIGN KEY (category_id) REFERENCES food_category(id)
);

-- mission: 미션 정보, 특정 가게(store)와 연결됨
CREATE TABLE mission (
    id BIGINT PRIMARY KEY AUTO_INCREMENT,
    store_id BIGINT,
    reward INT,
    deadline DATETIME,
    mission_spec TEXT,
    created_at DATETIME(6),
    updated_at DATETIME(6),
    FOREIGN KEY (store_id) REFERENCES store(id)
);

-- member_mission: 회원의 미션 수행 상태 (진행중/완료 등)
CREATE TABLE member_mission (
    id BIGINT PRIMARY KEY AUTO_INCREMENT,
    member_id BIGINT,
    mission_id BIGINT,
    status VARCHAR(15),
    created_at DATETIME(6),
    updated_at DATETIME(6),
    FOREIGN KEY (member_id) REFERENCES member(id),
    FOREIGN KEY (mission_id) REFERENCES mission(id)
);

-- review: 사용자 리뷰 정보 (가게와 회원 연결)
CREATE TABLE review (
    id BIGINT PRIMARY KEY AUTO_INCREMENT,
    member_id BIGINT,
    store_id BIGINT,
    body TEXT,
    score FLOAT,
    created_at DATETIME(6),
    FOREIGN KEY (member_id) REFERENCES member(id),
    FOREIGN KEY (store_id) REFERENCES store(id)
);

-- review_image: 리뷰에 포함된 이미지 URL
CREATE TABLE review_image (
    id BIGINT PRIMARY KEY AUTO_INCREMENT,
    review_id BIGINT,
    store_id BIGINT,
    image_url TEXT,
    created_at DATETIME(6),
    updated_at DATETIME(6),
    FOREIGN KEY (review_id) REFERENCES review(id),
    FOREIGN KEY (store_id) REFERENCES store(id)
);

-- member_agree: 회원이 어떤 약관에 동의했는지 기록
CREATE TABLE member_agree (
    id BIGINT PRIMARY KEY AUTO_INCREMENT,
    member_id BIGINT,
    terms_id BIGINT,
    created_at DATETIME(6),
    updated_at DATETIME(6),
    FOREIGN KEY (member_id) REFERENCES member(id),
    FOREIGN KEY (terms_id) REFERENCES terms(id)
);

-- terms 더미 데이터 삽입
INSERT INTO terms (title, body, optional, created_at, updated_at)
VALUES 
('Privacy Policy', 'Details about privacy.', TRUE, NOW(), NOW()), 
('Terms of Service', 'Details about service.', FALSE, NOW(), NOW());

-- 음식 카테고리 더미 데이터
INSERT INTO food_category (name, Column4)
VALUES 
('Pizza', 'Food'), 
('Burgers', 'Food');

-- 회원 정보 더미 데이터
INSERT INTO member (name, gender, age, address, spec_address, status, inactive_date, social_type, created_at, updated_at, email, point, phone_num)
VALUES 
('John Doe', 'Male', 28, '123 Main St', 'Suite 100', 'active', NULL, 'type1', NOW(), NOW(), 'john@example.com', 100, '010-1111-1111'),
('Jane Doe', 'Female', 25, '456 Oak St', 'Apt 200', 'active', NULL, 'type2', NOW(), NOW(), 'jane@example.com', 200, '010-1111-1111');

-- 지역 정보 더미 데이터
INSERT INTO region (name, created_at, updated_at) 
VALUES ('Seoul', NOW(), NOW()), ('Busan', NOW(), NOW());

-- 가게 정보 더미 데이터
INSERT INTO store (region_id, name, address, score, created_at, updated_at)
VALUES 
(1, 'Awesome Store', '789 Store Ave', 4.5, NOW(), NOW()), 
(2, 'Great Shop', '101 River Rd', 4.7, NOW(), NOW());

-- 미션 정보 더미 데이터
INSERT INTO mission (store_id, reward, deadline, mission_spec, created_at, updated_at)
VALUES 
(1, 500, '2024-12-31 23:59:59', 'Complete 5 purchases', NOW(), NOW()),
(2, 300, '2024-10-31 23:59:59', 'Write 2 reviews', NOW(), NOW());

-- 회원의 미션 상태 기록
INSERT INTO member_mission (member_id, mission_id, status, created_at, updated_at)
VALUES 
(1, 1, '진행중', NOW(), NOW()), 
(2, 2, '진행완료', NOW(), NOW());

-- 리뷰 데이터
INSERT INTO review (member_id, store_id, body, score, created_at)
VALUES 
(1, 1, 'Great experience!', 5.0, NOW()), 
(2, 2, 'Nice shop!', 4.8, NOW());

-- 리뷰 이미지 데이터
INSERT INTO review_image (review_id, store_id, image_url, created_at, updated_at)
VALUES 
(1, 1, 'http://example.com/image1.jpg', NOW(), NOW()), 
(2, 2, 'http://example.com/image2.jpg', NOW(), NOW());

-- 약관 동의 데이터
INSERT INTO member_agree (member_id, terms_id, created_at, updated_at)
VALUES 
(1, 1, NOW(), NOW()), 
(2, 2, NOW(), NOW());

-- 회원의 음식 선호 데이터
INSERT INTO member_prefer (member_id, category_id, created_at, updated_at)
VALUES 
(1, 1, NOW(), NOW()), 
(2, 2, NOW(), NOW());

-- 외래키 제약 다시 켜기
SET FOREIGN_KEY_CHECKS = 1;
```

---

### 📌 스크립트 실행 시 자동 처리 절차

1. 외래키 제약 해제 (`SET FOREIGN_KEY_CHECKS = 0;`)
2. 기존 테이블 제거 (존재 시)
3. 테이블 재생성 (`CREATE TABLE`)
4. 더미 데이터 삽입 (`INSERT`)
5. 외래키 제약 다시 활성화 (`SET FOREIGN_KEY_CHECKS = 1;`)

---

### 외래키(Foreign Key)

> 한 테이블의 컬럼이 다른 테이블의 기본키를 참조하도록 설정하는 제약 조건
> 

두 테이블 사이의 **관계(Relation)**를 정의하고 **데이터의 일관성과 무결성(정확성)**을 보장해주는 장치

예시: `store.region_id` → `region.id`

```sql
FOREIGN KEY (region_id) REFERENCES region(id)
```

→ **store 테이블의 region_id 값이 반드시 region 테이블의 id 값 중 하나여야 한다**는 뜻

즉, **없는 지역 ID는 저장 불가!**

---

### 외래키 제약이 걸려 있을 때 생기는 특징

| 상황 | 결과 |
| --- | --- |
| 없는 값을 참조하려 하면 | ❌ 에러 발생 |
| 참조당하는 테이블을 먼저 지우려고 하면 | ❌ 에러 발생 (`Cannot drop table ... referenced by foreign key`) |
| 연결된 데이터를 삭제하면 | ❌ 기본적으로 불가 (ON DELETE 옵션으로 제어 가능) |

---

### 외래키 제약을 끈 이유

**📌 DROP TABLE 시 순서 문제 때문!!!**

예를 들어 `member_mission` 테이블은 `member`와 `mission`을 참조하고 있을때,  `member` 테이블을 먼저 지우려고 하면…

➡ **참조당하고 있어서 지울 수 없음**

그래서 다음과 같은 에러가 발생:

```bash
Cannot drop table 'member' referenced by a foreign key constraint
```

---

### 해결 방법

**외래키 제약 조건 일시 해제**

```sql
SET FOREIGN_KEY_CHECKS = 0;
```

**→ 이걸 설정하면 외래키 제약 검사를 잠시 중단**해서 **참조 관계에 상관없이 자유롭게 DROP/CREATE 가능**해짐

**다시 외래키 검사 켜기**

```sql
SET FOREIGN_KEY_CHECKS = 1;
```

→ 이후에는 **다시 무결성 체크가 작동**돼서 잘못된 참조를 막아줌.

---

### 실무에서도 자주 쓰이는 상황

| 상황 | 외래키 제약 끄고 다시 켜는 이유 |
| --- | --- |
| 초기 데이터베이스 설계 및 마이그레이션 | 테이블 순서 상관없이 만들기 위해 |
| 테이블 전체 삭제 후 재생성 | 외래키 오류 없이 DROP 하기 위해 |
| 더미 데이터 넣기 | 순서 맞추기 귀찮을 때 일시적으로 끔 |

---

### ✅ 요약 정리

| 명령어 | 설명 |
| --- | --- |
| `SET FOREIGN_KEY_CHECKS = 0;` | 외래키 제약 조건 **잠시 끔** |
| `SET FOREIGN_KEY_CHECKS = 1;` | 외래키 제약 조건 **다시 켬** |
| 왜 쓰나? | 참조 순서 상관없이 테이블을 만들거나 지우기 위해 필요 |

---

### `DROP TABLE IF EXISTS 테이블명;` 코드의 목적

```sql
DROP TABLE IF EXISTS 테이블명;
```

> **→ "해당 테이블이 존재하면 삭제한다"**는 뜻
> 
> 
> 즉, 이미 만들어진 테이블이 있다면 삭제하고 새로 만들 준비를 하겠다는 의미
> 

---

### 테이블을 먼저 삭제한 이유

1. **초기화 목적**

- 테이블 구조나 컬럼을 수정하려면 보통 기존 테이블을 삭제한 뒤 다시 만들어야 함
- DB를 **깨끗한 초기 상태로 만들기 위해**

2. **스크립트를 반복 실행하기 위함**

- 실습할 때 또는 배포 자동화 시, 같은 SQL을 여러 번 실행해야 하는 경우가 많
- 그럴 때 기존 테이블이 있으면 `CREATE TABLE`에서 오류가 나기 때문에
    
    **먼저 DROP 하고 다시 CREATE 하는 게 안전함**
    

3. **외래키 제약을 고려한 순서대로 삭제**

- 외래키 관계가 있으므로, 참조하는 테이블보다 **참조당하는 테이블을 먼저 삭제해야 함**
- 예: `member_mission`은 `member`를 참조하니까 `member_mission`을 먼저 DROP해야 `member`를 삭제 가능

---

### IF EXISTS가 중요한 이유

- 만약 `DROP TABLE member;`만 쓴다면, 해당 테이블이 **없으면 에러 발생**
- 반면 `DROP TABLE IF EXISTS member;`는
    - 테이블이 있으면 삭제하고
    - 없으면 **조용히 넘어감 (에러 없음)**

> 그래서 실습이나 초기화 스크립트에는 항상 IF EXISTS를 붙여주는 게 좋음!
> 

---

### ✅ 요약

| 이유 | 설명 |
| --- | --- |
| 테이블 초기화 | 기존 테이블 삭제하고 새로 만들기 위해 |
| 반복 실행 가능 | 같은 SQL 스크립트를 여러 번 실행 가능하게 |
| 에러 방지 | 테이블이 없더라도 에러 없이 넘어가게 |
| 외래키 순서 고려 | 참조 관계 순서대로 안전하게 삭제 가능 |

---
