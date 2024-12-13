---
layout: post
title: Spring Boot @SpringQueryMap
categories: [일상 중 궁금했던]
comments: true
---

### Feign Client 어노테이션이 붙은 클래스 내에 처음 보는 어노테이션이 있어서 찾아봄

## 개요
`@SpringQueryMap`은 Spring Cloud OpenFeign에서 **쿼리 파라미터를 객체로 변환하여 전달**할 때 사용하는 어노테이션입니다.

## 주요 기능
- `@RequestParam` 대신 객체로 쿼리 파라미터를 한 번에 처리 가능
- `FeignClient`에서 사용할 때 가독성을 높이고 코드 간결화 가능

## 사용 예시

### DTO 정의
```java
public class UserQuery {
    private String name;
    private int age;
    
    // Getter & Setter
}
```

### Feign Client에서 사용
```java
@FeignClient(name = "userClient", url = "https://api.example.com")
public interface UserClient {
    @GetMapping("/users")
    List<UserDTO> getUsers(@SpringQueryMap UserQuery query);
}
```

### 변환 예시
요청 객체:
```java
UserQuery query = new UserQuery();
query.setName("Alice");
query.setAge(25);
```

실제 전송되는 요청:
```
GET /users?name=Alice&age=25
```

## 장점
- 코드의 가독성이 향상됨
- 여러 개의 쿼리 파라미터를 한 객체로 처리 가능

## 주의사항
- `@SpringQueryMap`은 **Spring Cloud OpenFeign에서만 동작**하며, 일반 Spring MVC에서는 사용할 수 없음



