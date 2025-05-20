**성공 응답**

**1. [User] 회원가입**

![Image](https://github.com/user-attachments/assets/72a1c851-f2a3-4448-b33f-3a08c39c1753)

**2. [Store] 가게 추가**

![Image](https://github.com/user-attachments/assets/1c53b269-12b4-4b4c-ab0e-8ff69a5f14e5)

**3-1. [Mission] 미션 추가**

![Image](https://github.com/user-attachments/assets/14038618-ca15-42d4-9320-53d3dec5d32d)

**3-2. [Mission] 미션 도전**

![Image](https://github.com/user-attachments/assets/63ed220b-24fa-402a-bddf-a62483b877e1)

**3-3. [Mission] 미션 완료 처리**

![Image](https://github.com/user-attachments/assets/efe8b3fb-84bc-416b-8d09-ca954dcb3b9c)

4. **[Review] 리뷰 추가 및 조회**

![Image](https://github.com/user-attachments/assets/f700d86e-1c60-4ced-9184-15a1dcfdcb4c)

---

**에러 발생**

**1. [User] 회원가입**

U001
회원가입 시 사용자가 입력한 이메일이 DB에 이미 존재 → 중복 등록 방지

![Image](https://github.com/user-attachments/assets/c3a8f654-ceaf-43e5-80a3-850259717b13)

---

U002 
사용자 입력 중 필수 값(email, phoneNumber 등)이 누락되었거나, 형식이 잘못됨 → 클라이언트 측의 요청 문제를 명확히 구분

![Image](https://github.com/user-attachments/assets/0fe7219f-e9b2-425b-aff0-5fc65fe7e18c)

---

**2. [Store] 가게 추가**

S001

가게 추가 요청 시 regionName이나 storeName이 누락 → 필수값 확인을 통해 클라이언트 요청 오류를 처리

![Image](https://github.com/user-attachments/assets/96582c14-2a6a-4840-b084-0367363f4599)

---

3. **[Mission] 미션 추가 및 도전**

M001

미션 생성 또는 도전 요청 시 storeId, name, reward 등의 필수 값이 누락

![Image](https://github.com/user-attachments/assets/047cc2e8-d9d1-4944-98f4-3fa950df001d)

---

M002

사용자가 동일한 미션에 이미 도전 중인 상태에서 다시 도전하려는 경우  → 중복 도전을 막기 위해 사용

![Image](https://github.com/user-attachments/assets/4db19537-d602-4687-ba60-f14a9eafe4ee)

---

M003
사용자가 해당 미션에 진행 중인 상태가 아닌데 완료 처리를 시도한 경우 발생 → 없는 도전 완료 방지

![Image](https://github.com/user-attachments/assets/d43e00ec-9b3c-4cff-824e-841f00d93bcb)

---

4. **[Review] 리뷰 추가 및 조회**

R001

리뷰 작성 시 점수(score)나 내용(body) 등이 누락되었을 때 발생 → 입력값 유효성 검사를 위해 사용

![Image](https://github.com/user-attachments/assets/08f984de-60ff-4cae-99f9-ec6008c51f14)

---

R002

리뷰 작성 시 연결된 가게(storeId)가 DB에 존재하지 않는 경우 → 논리적 오류 방지를 위한 처리
![Image](https://github.com/user-attachments/assets/f80afcba-1735-4095-8148-cb6a3652538c)
