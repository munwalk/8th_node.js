1. 특정 지역에 가게 추가하기 API
![Image](https://github.com/user-attachments/assets/83226a78-6318-4f4c-8f2c-04866e5eeb96)

2. **가게에 리뷰 추가하기 API**
    - 리뷰를 추가하려는 가게가 존재하는지 검증이 필요합니다.
![Image](https://github.com/user-attachments/assets/4d3bad8e-bcaa-422a-94fe-ae5c333421f8)

3. 가게에 미션 추가하기 API
![Image](https://github.com/user-attachments/assets/48177c5c-5ca4-4882-b327-4c7f4a72df13)

4. **가게의 미션을 도전 중인 미션에 추가(미션 도전하기) API**
![Image](https://github.com/user-attachments/assets/d5464cf4-4854-4cb7-bad8-640c82ad7b25)
![Image](https://github.com/user-attachments/assets/d0f8362d-6d59-4a4d-935f-3521880b59a6)

---
- 미션 기록
    
    ---
    
    1. **특정 지역에 가게 추가하기 API**
        
        ➊ API URL
        
        POST /api/v1/stores
        
        - 새로운 가게를 등록 → **POST** 메소드 사용
        - 리소스는 **store**(가게) 중심 → `/stores` 사용
        
        ---
        
        ➋ HTTP 메소드
        
        POST
        
        - 새로운 데이터를 **등록(생성)** → POST
        
        ---
        
        ➌ 요청 데이터 (Request Body)
        
        ```json
        {
          "regionName": "광진구",
          "storeName": "롯데리아"
        }
        ```
        
        - **regionName**: 가게가 위치할 지역
        - **storeName**: 추가할 가게 이름
        
        ---
        
        ➍ 검증 로직
        
        - 같은 **regionName + storeName** 조합의 가게가 이미 존재 → **409 Conflict** 에러를 응답 & `"이미 등록된 가게입니다."` 메시지 보냄
        
    
    ---
    
    1. **가게에 리뷰 추가하기 API(리뷰를 추가하려는 가게가 존재하는지 검증이 필요)**
        
        ➊ API URL
        
        POST /api/v1/reviews
        
        - 리뷰는 특정 가게(store)에 대해 작성 → `/reviews`로 설계
        
        ---
        
        ➋ HTTP 메소드
        
        POST
        
        - 새 리뷰를 데이터베이스에 등록 → POST
        
        ---
        
        ➌ 요청 데이터 (Body)
        
        ```json
        {
          "memberId": 1,
          "storeId": 1,
          "body": "진짜 최고였어요!",
          "score": 4.5
        }
        ```
        
        - memberId : 리뷰를 작성하는 사용자 ID (DB에 저장된 사용자)
        - storeId : 리뷰를 작성할 가게 ID (DB에 저장된 가게)
        - body : 리뷰 본문
        - score : 리뷰 점수 (1.0 ~ 5.0 범위의 실수값)
        
        ---
        
        ➍ 검증 로직
        
        - 리뷰를 등록하기 전에 **storeId에 해당하는 가게가 실제 존재하는지** DB에서 먼저 조회해서 검증해야 함
        - 만약 해당 가게가 존재하지 않는 경우 → **400 Bad Request** 응답을 주고, 메시지는 `"존재하지 않는 가게입니다."`로 보냄
        - 가게가 존재할 경우에만 리뷰를 정상적으로 추가
    
    ---
    
    1. **가게에 미션 추가하기 API**
        
        ➊ API URL
        
        POST /api/v1/missions/challenge
        
        - **새로운 미션 등록 →** POST 메소드
        - 리소스는 **mission** 중심 →  `/missions` 사용
        
        ---
        
        ➋ HTTP 메소드
        
        POST
        
        - 새로운 데이터를 등록(생성)하는 작업 → POST
        
        ---
        
        ➌ 요청 데이터 (Request Body)
        
        ```json
        {
          "storeId": 1,
          "name": "첫 번째 미션",
          "reward": "아이스크림 쿠폰",
          "missionSpec": "음료 2개 이상 구매하기"
        }
        ```
        
        | 필드명 | 설명 |
        | --- | --- |
        | storeId | 미션이 소속될 가게 ID (DB에 저장된 가게) |
        | name | 미션 이름 |
        | reward | 미션 성공 시 받을 보상 이름 (ex. '아이스크림 쿠폰') |
        | missionSpec | 미션 수행 조건 설명 |
        
        ---
        
        ➍ 검증 로직
        
        - 요청된 **storeId**에 해당하는 가게가 존재하는지 먼저 검증
        - 가게가 존재하지 않으면 **400 Bad Request** 응답하고, `"존재하지 않는 가게입니다."` 메시지를 보냄
    
    ---
    
    1. **가게의 미션을 도전 중인 미션에 추가(미션 도전하기) API**
        - 도전하려는 미션이 이미 도전 중이지는 않은지 검증이 필요합니다.
        - 3번 API를 구현하지 않은 경우, 4번에서는 DB에 미션 정보를 수동으로 기입한 후 진행해야 합니다.
        
        ➊ API URL
        
        POST /api/v1/missions/challenge
        
        - 사용자가 미션에 "도전"하는 요청이라 별도 `/missions/challenge` 엔드포인트 사용
        
        ---
        
        ➋ HTTP 메소드
        
        POST
        
        - 새로운 도전 기록을 등록(생성)하는 작업 →  POST
        
        ---
        
        ➌ 요청 데이터 (Request Body)
        
        ```json
        {
          "memberId": 1,
          "missionId": 1
        }
        ```
        
        | 필드명 | 설명 |
        | --- | --- |
        | memberId | 미션을 도전하는 사용자 ID (DB에 저장된 사용자) |
        | missionId | 도전하려는 미션 ID (DB에 저장된 미션) |
        
        ---
        
        ➍ 검증 로직
        
        - 요청이 들어오면 **memberId와 missionId** 기준으로 **이미 진행 중인 도전 기록이 있는지** 먼저 확인해야 함
        - 이미 존재할 경우, `409 Conflict` 에러와 함께, `"이미 도전 중인 미션입니다."` 메시지 보냄
        - 존재하지 않는다면 새로운 도전 기록(`진행중`) 추가
