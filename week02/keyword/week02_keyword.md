# 2주차 키워드😎

## JOIN
- (INNER) JOIN : 조인하는 테이블의 ON 절의 조건이 일치하는 결과만 출력
- LEFT JOIN : 두 테이블이 있을 경우, 첫 번째 테이블을 기준으로 두 번째 테이블을 조합하는 JOIN
  - → 좌측 테이블 데이터에 추가로 우측 정보를 조인
  - 가장 첫 번째의 테이블로 SELECT문에 가장 많은 열을 가져와야 할 테이블을 우선으로 적어준다.
  - 시작을 LEFT JOIN으로 했다면 나머지 조인도 LEFT JOIN을 이어나간다.
  ```
  FROM A
  LEFT JOIN B ON 조건
  ```
  -> A를 기준으로, B는 조건 맞으면 붙고 안 맞으면 NULL, A는 조건 상관없이 무조건 결과에 포함
  ```
  SELECT photos.filename, users.nickname
  FROM photos
  LEFT JOIN users ON users.id = photos.user_id;
  ```
  - `LEFT JOIN users ON users.id = photos.user_id`**→** 사진 올린 사람의 정보를 연결해주는 JOIN
  - `users.id = photos.user_id`: 사진을 업로드한 사람이 누구인지 연결하는 조건
  - `LEFT JOIN`: 만약 `user_id`에 해당하는 사용자가 `users` 테이블에 **없더라도**, 사진은 **무조건 보여준다**는 뜻!

## 서브쿼리
### 다른 쿼리 내부에 포함되어 있는 SELETE 문

- 서브쿼리(=자식쿼리, 내부쿼리) - 메인쿼리 컬럼 사용 가능
- 메인쿼리(=부모쿼리, 외부쿼리) - 서브쿼리 컬럼 사용 불가
  
- 스칼라 서브쿼리(Scalar Sub Query): 하나의 컬럼처럼 사용 (표현 용도)
    
    SELECT col1, (SELECT ...)
    
- 인라인 뷰(Inline View): 하나의 테이블처럼 사용 (테이블 대체 용도)
    
    FROM (SELECT ...)
    
- 일반 서브쿼리: 하나의 변수(상수)처럼 사용 (서브쿼리의 결과에 따라 달라지는 조건절)
    
    WHERE col = (SELECT ...)
