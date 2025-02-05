---
layout: post
title: DTO와 Domain의 차이
categories: [일상 중 궁금했던]
comments: true
---

# DTO vs Domain(Entity, Model)

## DTO (Data Transfer Object)
### 목적
- 계층 간(Controller ↔ Service ↔ Repository) 데이터 전달

### 특징
- 단순한 데이터 구조, 비즈니스 로직 없음
- 직렬화/역직렬화에 적합
- API 응답 형식에 맞춰 가공 가능

### 예제 (Kotlin)
```kotlin
data class UserDTO(
    val id: Long,
    val name: String,
    val email: String
)
```

### 사용처
- API Request/Response
- 엔터티를 가공하여 반환할 때
- 여러 개의 엔터티 데이터를 조합할 때

---

## Domain (Entity, Model)
### 목적
- 데이터베이스 테이블과 매핑, 비즈니스 로직 포함

### 특징
- 데이터의 상태 관리, CRUD 작업 수행
- ORM과 함께 사용됨
- 관계(Relation) 설정 가능

### 예제 (Kotlin + Exposed ORM)
```kotlin
object Users : Table() {
    val id = long("id").autoIncrement()
    val name = varchar("name", 255)
    val email = varchar("email", 255)
    override val primaryKey = PrimaryKey(id)
}

data class User(
    val id: Long?,
    val name: String,
    val email: String
)
```

### 사용처
- 데이터베이스와의 CRUD 작업
- 데이터의 상태 관리
- 서비스 로직에서 직접 활용

---

## DTO vs Domain 비교
| 비교 항목 | DTO | Domain (Entity, Model) |
|-----------|----|----------------------|
| 목적 | 데이터 전달 | 비즈니스 로직 및 데이터 관리 |
| 구조 | 단순한 데이터 구조 | 데이터 + 비즈니스 로직 포함 가능 |
| 관계 | 다른 DTO와 관계 없음 | 다른 엔터티와 연관관계 가능 |
| 사용 위치 | Controller ↔ Service ↔ API | Repository ↔ Service |
| ORM 사용 | X | O (ORM과 매핑) |
| 가변성 | 일반적으로 불변 (`val`) | 가변 (`var`) 가능 |

---

## DTO와 Domain 변환 예제
```kotlin
data class User(
    val id: Long?,
    val name: String,
    val email: String
) {
    fun toDTO(): UserDTO {
        return UserDTO(id ?: 0, name, email)
    }
}

data class UserDTO(
    val id: Long,
    val name: String,
    val email: String
)

val user = User(1, "John Doe", "john@example.com")
val userDTO = user.toDTO()  // Entity -> DTO 변환
```

---

## DTO 사용 사례
1. API 응답에서 Entity를 직접 노출하면 안 될 때
2. 데이터를 변형해서 응답해야 할 때 (예: 여러 엔터티 조합)
3. 서비스 계층의 책임을 분리하고 싶을 때


