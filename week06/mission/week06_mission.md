## 실습인증
1. 내가 작성한 리뷰 목록
   ![Image](https://github.com/user-attachments/assets/513f429f-b087-491d-834f-934f9a164493)
   
2. 특정 가게의 미션 목록
    ![Image](https://github.com/user-attachments/assets/ec6ab563-e217-4b67-8089-145d6c236f80)
   
3. 내가 진행 중인 미션 목록
   ![Image](https://github.com/user-attachments/assets/faf51765-a3f0-42d9-a96b-a779f03781b6)
    
4. 내가 진행 중인 미션을 진행 완료로 바꾸기
   ![Image](https://github.com/user-attachments/assets/04fe0e89-85d6-4428-a081-3c9e5e4a1558)
   ![Image](https://github.com/user-attachments/assets/47072f6b-b374-445b-af83-6c85e9e7ea9b)

---

## 미션기록

- Repository 함수 리팩토링 (SQL → Prisma ORM)
    
    
    | 항목 | 5주차 (직접 SQL) | 6주차 (Prisma ORM) |
    | --- | --- | --- |
    | 리뷰 등록 | `INSERT INTO review...` | `prisma.userStoreReview.create()` |
    | 가게 등록 | `INSERT INTO store...` | `prisma.store.create()` |
    | 미션 등록 | `INSERT INTO mission...` | `prisma.mission.create()` |
    | 미션 도전 | `INSERT INTO member_mission...` | `prisma.memberMission.create()` |
    | 중복 도전 확인 | `SELECT * FROM member_mission...` | `prisma.memberMission.findFirst()` |
    | 리뷰 목록 조회 | `SELECT * FROM review...` | `prisma.userStoreReview.findMany()` |

---

1. **내가 작성한 리뷰 목록**
    
    🔹 기존 (mysql2/promise 방식):
    
    ```jsx
    const conn = await pool.getConnection();
    const [rows] = await conn.query(
      "SELECT * FROM user_store_review WHERE user_id = ?",
      [userId]
    );
    conn.release();
    return rows;
    ```
    
    🔹 변경 후 (Prisma ORM):
    
    ```jsx
    const reviews = await prisma.userStoreReview.findMany({
      where: { userId },
    });
    return reviews;
    ```
    
    > userStoreReview 테이블에서 userId로 필터링
    SQL의 WHERE 절을 Prisma의 where 필드로 대체
    > 
    
    🔄 변경 이유 및 장점
    
    | 항목 | 설명 |
    | --- | --- |
    | 직접 쿼리 작성 | SQL 문법 에러 가능성, 유지보수 어려움 |
    | Prisma 사용 | `userStoreReview` 모델에 기반한 안전한 조회 가능 |
    | 코드 간결성 | 조건만 적으면 되므로 복잡한 연결 생략 가능 |

---

2. **특정 가게의 미션 목록**
    
    🔹 기존:
    
    ```jsx
    const conn = await pool.getConnection();
    const [missions] = await conn.query(
      "SELECT * FROM mission WHERE store_id = ?",
      [storeId]
    );
    conn.release();
    return missions;
    ```
    
    🔹 변경 후:
    
    ```jsx
    const missions = await prisma.mission.findMany({
      where: { storeId },
    });
    return missions;
    ```
    
    > 해당 가게(storeId)에 등록된 미션 목록 조회
    역시 `where` 필터로 전환
    > 
    
    🔄 변경 이유 및 장점
    
    | 항목 | 설명 |
    | --- | --- |
    | raw query | DB 구조 변경 시 쿼리도 같이 수정해야 함 |
    | Prisma | `storeId`가 외래키로 설정된 구조를 코드로 안전하게 조회 가능 |
    | 안전성 증가 | relation 기반 쿼리로 참조 무결성 보장 |

---

3. **내가 진행 중인 미션 목록**
    
    🔹 기존:
    
    ```jsx
    const conn = await pool.getConnection();
    const [missions] = await conn.query(
      "SELECT * FROM member_mission WHERE member_id = ? AND status = '진행중'",
      [memberId]
    );
    conn.release();
    return missions;
    ```
    
    🔹 변경 후:
    
    ```jsx
    const missions = await prisma.memberMission.findMany({
      where: {
        memberId,
        status: '진행중',
      },
    });
    return missions;
    ```
    
    > member_mission 테이블에서 특정 사용자의 `진행중` 상태 미션 필터링
    > 
    
    🔄 변경 이유 및 장점
    
    | 항목 | 설명 |
    | --- | --- |
    | SQL 반복문 | 같은 쿼리 패턴 반복됨 |
    | Prisma 조건절 | `where` 조건으로 status 필터링 |
    | 타입 안정성 | Prisma schema에 명시된 타입 기반 쿼리라 에러 가능성 낮음 |

---

4. **내가 진행 중인 미션을 진행 완료로 바꾸기**
    
    🔹 기존:
    
    ```jsx
    const conn = await pool.getConnection();
    await conn.query(
      "UPDATE member_mission SET status = '완료' WHERE id = ?",
      [missionId]
    );
    conn.release();
    ```
    
    🔹 변경 후:
    
    ```jsx
    await prisma.memberMission.update({
      where: { id: missionId },
      data: { status: '완료' },
    });
    ```
    
    > Prisma에서 updateMany는 조건 일치 row들을 찾아 `data`로 업데이트
    SQL의 `UPDATE ... WHERE ...`과 유사한 구조
    > 
    
    🔄 변경 이유 및 장점
    
    | 항목 | 설명 |
    | --- | --- |
    | SQL 에러 처리 직접 구현 | try-catch 및 rollback 필요 |
    | Prisma update | 에러 발생 시 명확한 메시지 제공 및 롤백 기능 |
    | 가독성 ↑ | update 쿼리 직관적 구성 (`where`, `data` 분리) |

---

- **schema.prisma**
    
    `Prisma ORM`의 핵심 설정 파일로, **데이터베이스 구조를 정의**하는 역할
    
    즉, ERD(엔티티 관계도)를 코드로 작성한 것과 동일
    
    주요 구성:
    
    | 구성 요소 | 설명 |
    | --- | --- |
    | `generator` | Prisma Client 코드 자동 생성 설정 |
    | `datasource` | 어떤 DB를 사용할지, 연결 정보(`.env`의 `DATABASE_URL`)를 명시 |
    | `model` | 하나의 테이블에 해당. 각 모델은 컬럼과 관계(`@relation`)를 가짐 |

---

- **seed.js**
    
    `Prisma` 프로젝트에서 사용하는 **더미 데이터 삽입 스크립트**
    
    `npx prisma db seed` 명령으로 실행되며, **초기 테스트용 데이터를 DB에 삽입**
    
    사용 목적:
    
    - 테스트용 유저, 가게, 리뷰, 미션 등의 데이터를 자동 삽입
    - API 테스트 시 유의미한 샘플 데이터 제공
