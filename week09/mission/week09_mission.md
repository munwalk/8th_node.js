1. **네이버 Passport 라이브러리**
    
    ![Image](https://github.com/user-attachments/assets/8231c6cf-502a-4c34-9e4b-8fb00759ced1)
    
    ![Image](https://github.com/user-attachments/assets/e2ccd5c7-81e0-4082-abde-889f286e56f2)
    
    ![Image](https://github.com/user-attachments/assets/bf133913-6923-4b5d-8c36-123f4c448be3)
    

---

2. **기존에 사용자의 정보를 하드 코딩 했던 부분 수정**
    1. mission.controller.js
        
        1. `handleChallengeMission`
        
        기존:
        
        ```jsx
        const { memberId, missionId } = req.body;
        ```
        
        수정:
        
        ```jsx
        const userId = req.user?.id;
        const { missionId } = req.body;
        if (!userId || !missionId) {
          return res.status(400).error({ reason: "로그인 정보와 missionId가 필요합니다." });
        }
        const challengeId = await challengeMission({ memberId: userId, missionId });
        ```
        
        ---
        
        2. `handleCompleteMission`
        
        기존:
        
        ```jsx
        const { memberId, missionId } = req.body;
        const result = await completeMyMission({ memberId, missionId });
        ```
        
        수정:
        
        ```jsx
        const userId = req.user?.id;
        const { missionId } = req.body;
        if (!userId || !missionId) {
          return res.status(400).error({ reason: "로그인 정보와 missionId가 필요합니다." });
        }
        const result = await completeMyMission({ memberId: userId, missionId });
        ```
        
    
    ---
    
    b. review.controller.js
    
    1. `handleAddReview`
    
    **기존:**
    
    ```jsx
    const review = await addReviewService(req.body);
    ```
    
    **수정:**
    
    ```jsx
    const userId = req.user?.id;
    if (!userId) {
      return res.status(401).error({ reason: "로그인이 필요합니다." });
    }
    const review = await addReviewService({ ...req.body, userId });
    ```
    
    ---
    
    2. `handleListMyReviews`
    
    **기존:**
    
    ```jsx
    const { memberId } = req.params;
    const reviews = await listMyReviewsService(parseInt(memberId));
    
    ```
    
    **수정:**
    
    ```jsx
    const userId = req.user?.id;
    if (!userId) {
      return res.status(401).error({ reason: "로그인이 필요합니다." });
    }
    const reviews = await listMyReviewsService(userId);
    ```
    
    ---
    
    c. store.controller.js
    
    `store.controller.js`는 사용자 ID를 하드코딩해서 사용하지 않기 때문에 **수정 필요 없음**
    
    ---
    
    d. user.controller.js
    
    1. `handleUpdateMyInfo`
    
    **기존:**
    
    ```jsx
    const user = req.user;
    if (!user) {
      return res.status(401).error({ reason: "로그인이 필요합니다." });
    }
    const updatedUser = await prisma.user.update({
      where: { id: user.id },
      ...
    });
    ```
    
    **수정 필요 없음**
    
    이미 `req.user.id`를 사용하여 로그인 사용자 정보를 기준으로 작동하고 있음
    

---

3. **자기 자신의 정보를 수정하는 API 추가**

API 개요

| 항목 | 내용 |
| --- | --- |
| URL | `PATCH /api/v1/users/me` |
| 인증 | 세션 기반 (`req.user.id`) |
| 목적 | 소셜 로그인한 사용자가 자신의 상세 정보를 수정 |
| 요청 | `name`, `gender`, `birth`, `address`, `detailAddress`, `phoneNumber` |
| 응답 | 수정된 사용자 정보 반환 |

![Image](https://github.com/user-attachments/assets/47d2e3e2-834c-4f19-9f26-8f8f44b14c98)

```json
{
  "resultType": "SUCCESS",
  "error": null,
  "success": {
    "message": "사용자 정보가 성공적으로 수정되었습니다.",
    "user": {
      "id": 12,
      "email": "user@example.com",
      "name": "홍길동",
      "gender": "남성",
      "birth": "1990-01-01",
      "address": "서울시 강남구",
      "detailAddress": "테헤란로 123",
      "phoneNumber": "010-1234-5678"
    }
  }
}
```

---
