# Chapter 2. 실전 SQL - 어떤 Query를 작성해야 할까?🤷‍♀️

## 내가 진행중, 진행 완료한 미션 모아서 보는 쿼리(페이징 포함)
```
SELECT
    mm.id AS member_mission_id,
    mm.status,
    m.reward,
    m.mission_spec,
    s.name AS store_name,
    mm.created_at,
    mm.updated_at
FROM member_mission mm
JOIN mission m ON mm.mission_id = m.id    -- 유저가 수행한 미션 정보 연결
JOIN store s ON m.store_id = s.id         -- 미션이 속한 가게 정보 연결
-- 특정 유저의 미션만 조회
WHERE mm.member_id = :member_id           -- :member_id는 변수
  AND mm.status IN ('진행중', '성공')      -- 진행중/성공 상태만 조회
ORDER BY mm.updated_at DESC               -- 최근 미션 순서로 정렬
LIMIT 10 OFFSET 0;                        -- 페이징 처리 (10개씩, 0부터 시작)
```

## 리뷰 작성하는 쿼리(* 사진의 경우는 일단 배제)
```
-- review 테이블에 새로운 리뷰 데이터 삽입
INSERT INTO review (
    member_id,
    store_id,
    body,
    score,
    created_at,
    updated_at
)
-- 위에 지정한 컬럼들에 실제 값을 넣는 부분
VALUES (
    :member_id,
    :store_id,
    :body,
    :score,
    NOW(),
    NOW()
);
```

## 홈 화면 쿼리(현재 선택 된 지역에서 도전이 가능한 미션 목록, 페이징 포함)
```
SELECT
    m.id AS mission_id,
    m.reward,
    m.mission_spec,
    m.deadline,
    s.name AS store_name,
    s.address
FROM mission m
JOIN store s ON m.store_id = s.id         -- 미션-가게 연결
WHERE s.region_id = :region_id            -- 현재 선택된 지역의 미션만 조회
  AND m.id NOT IN (
      SELECT mission_id
      FROM member_mission
      WHERE member_id = :member_id        -- 유저가 이미 도전한 미션은 제외
  )
ORDER BY m.deadline ASC                   -- 마감일이 가까운 순서대로 정렬
LIMIT 10 OFFSET 0;
```

## 마이 페이지 화면 쿼리
```
SELECT
    id,
    name AS nickname,
    email,
    phone_number,          -- 휴대폰 번호 (NULL이면 미인증)
    point
FROM member
WHERE id = :member_id;     -- 현재 로그인한 유저 ID
```
