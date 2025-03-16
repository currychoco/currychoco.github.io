
---
layout: post
title: Kotlin class vs data class
categories: [일상 중 궁금했던]
comments: true
---

### data class의 경우 생성자 없이 사용되길래 궁금해서 찾아봄

## class
- 일반적인 클래스 정의
- `toString()`, `equals()`, `hashCode()`, `copy()` 자동 생성 없음
- 필요하면 직접 메서드 구현

```kotlin
class Person(val name: String, val age: Int)
```

## data class
- 데이터 저장을 위한 클래스
- `toString()`, `equals()`, `hashCode()`, `copy()` 자동 제공
- 최소 한 개 이상의 프로퍼티 필요
- `copy()` 메서드로 값 일부 변경 가능

```kotlin
data class Person(val name: String, val age: Int)
```

## 차이점 요약
| 구분       | class | data class |
|------------|-------|------------|
| 자동 메서드 | X     | O          |
| 주 용도     | 기능 구현 | 데이터 저장 |
| 객체 복사   | 직접 구현 | `copy()` 제공 |
