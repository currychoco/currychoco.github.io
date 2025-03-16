---
layout: post
title: Kotlin Data Class 상속 가능 여부
categories: [일상 중 궁금했던]
comments: true
---

### 1. Data Class는 Data Class를 상속받을 수 없다.
- Kotlin에서는 `data class`가 다른 `data class`를 상속받을 수 없음.
- 이유: `equals`, `hashCode`, `toString`, `copy` 등의 자동 생성 메서드와 상속 간의 충돌 방지.

### 2. 일반 클래스나 인터페이스 상속은 가능하다.
```kotlin
open class BaseClass(val id: Int)

data class DerivedClass(val name: String, val age: Int, id: Int) : BaseClass(id)
```
```kotlin
interface Printable {
    fun print()
}

data class PrintableClass(val name: String) : Printable {
    override fun print() {
        println("Name: $name")
    }
}
```

### 3. Data Class 재사용을 위한 대안
- **구성(Composition) 사용**
```kotlin
data class BaseData(val id: Int, val timestamp: Long)

data class ExtendedData(val name: String, val base: BaseData)
```
- 별도의 일반 클래스를 만들어 공통 로직 포함.

### 4. 결론
- Data Class는 다른 Data Class를 상속할 수 없지만, 일반 클래스나 인터페이스는 상속 가능.
- 코드 재사용을 위해 구성(Composition) 방식 활용 추천.

