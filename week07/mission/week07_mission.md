**성공 응답**

**1. [User] 회원가입**

![Image](https://github.com/user-attachments/assets/e4684f23-7ea0-4349-8b00-579ba9816b52)

**2. [Store] 가게 추가**

![Image](https://github.com/user-attachments/assets/6fa14022-5a51-478c-811f-f54b8d82c77f)

**3-1. [Mission] 미션 추가**

![Image](https://github.com/user-attachments/assets/39ef198c-13bb-4045-ad67-19a818647728)

**3-2. [Mission] 미션 도전**

![Image](https://github.com/user-attachments/assets/f8019aaa-0958-4731-a18c-543a25c7ba0b)

**3-3. [Mission] 미션 완료 처리**

![Image](https://github.com/user-attachments/assets/2106484f-58cb-49dd-a628-1d1d3a717f40)

4. **[Review] 리뷰 추가 및 조회**

![Image](https://github.com/user-attachments/assets/3878a3f9-e552-46de-8ca9-dc0ebd0579f4)

---

**에러 발생**

**1. [User] 회원가입**

U001

회원가입 시 사용자가 입력한 이메일이 DB에 이미 존재 → 중복 등록 방지

![Image](https://github.com/user-attachments/assets/26226412-3d65-4ea5-8c1a-777d3e67989b)

---

U002 

사용자 입력 중 필수 값(email, phoneNumber 등)이 누락되었거나, 형식이 잘못됨 → 클라이언트 측의 요청 문제를 명확히 구분

![Image](https://github.com/user-attachments/assets/50695234-a9ba-4a0b-a884-e0972caca7b8)

---

**2. [Store] 가게 추가**

S001

가게 추가 요청 시 regionName이나 storeName이 누락 → 필수값 확인을 통해 클라이언트 요청 오류를 처리

![Image](https://github.com/user-attachments/assets/d1b0cdd3-e8a1-4436-a1ba-5edaf477a75a)

---

3. **[Mission] 미션 추가 및 도전**

M001

미션 생성 또는 도전 요청 시 storeId, name, reward 등의 필수 값이 누락

![Image](https://github.com/user-attachments/assets/5f501cd0-029b-4a11-a27d-3f5dfad6050f)

---

M002

사용자가 동일한 미션에 이미 도전 중인 상태에서 다시 도전하려는 경우  → 중복 도전을 막기 위해 사용

![Image](https://github.com/user-attachments/assets/d144706a-aaee-4ff3-bf46-1c9f70074008)

---

M003

사용자가 해당 미션에 진행 중인 상태가 아닌데 완료 처리를 시도한 경우 발생 → 없는 도전 완료 방지

![Image](https://github.com/user-attachments/assets/45c93297-e644-4a97-9f61-bda274e4c524)

---

4. **[Review] 리뷰 추가 및 조회**

R001

리뷰 작성 시 점수(score)나 내용(body) 등이 누락되었을 때 발생 → 입력값 유효성 검사를 위해 사용

![Image](https://github.com/user-attachments/assets/ab72b742-a304-4c03-9e08-dc7706b49ce3)

---

R002

리뷰 작성 시 연결된 가게(storeId)가 DB에 존재하지 않는 경우 → 논리적 오류 방지를 위한 처리

![Image](https://github.com/user-attachments/assets/2817b421-cf11-4a36-9d83-e8a941ea0dbe)

---
