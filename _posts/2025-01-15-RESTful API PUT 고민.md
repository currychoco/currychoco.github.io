---
layout: post
title: RESTful API PUT 고민
categories: [일상 중 궁금했던]
comments: true
---

### 어차피 DTO에 id가 들어 있는데 굳이 pathvariable로 id를 표시해야할까?

### RESTful 원칙에 따른 URL 설계
- **PUT 메서드는 특정 리소스 전체를 업데이트**하는 것이므로, URL에서 명확하게 리소스를 식별해야 함.
- 일반적인 RESTful API 패턴: `PUT /brands/{id}`
- URL에 ID를 포함하면 API의 명확성과 일관성을 유지할 수 있음.

### URL에 ID를 포함해야 하는 이유
1. **리소스를 명확하게 식별** → 클라이언트가 어떤 리소스를 수정하는지 URL만 보고 알 수 있음.
2. **HTTP 메서드의 의미 유지** → `POST /brands` (생성), `PUT /brands/{id}` (수정) 일관성 유지.
3. **다른 RESTful API와의 호환성** → RESTful 원칙을 준수하여 클라이언트와 서버 간의 명확한 계약 형성.

### DTO에 ID가 있는 경우 URL에 ID를 넣어야 할까?
- **넣는 것이 일반적** → URL에서 리소스를 식별하고, 요청 본문에서도 ID를 포함.
- **ID 검증 필요** → URL의 ID와 DTO의 ID가 일치하는지 확인하는 것이 안전함.

### ID를 URL에 포함하지 않는 경우
- `PUT /brands` + 요청 본문에 ID 포함 방식도 가능하지만, RESTful 원칙에서는 벗어남.
- API의 명확성이 떨어지고, URL만 보고 어떤 리소스를 수정하는지 알기 어려움.
- 특정 조건으로 업데이트할 때는 `PATCH`를 사용하는 것이 적절할 수 있음.

### 결론
- RESTful 원칙을 준수하려면 `PUT /brands/{id}` 형식이 가장 적절함.
- DTO에 ID가 있어도 URL에 ID를 포함하여 리소스를 명확하게 식별하는 것이 좋음.
- ID 검증 로직을 추가하여 안전성을 높이는 것이 바람직함.

