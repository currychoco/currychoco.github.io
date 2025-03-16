---
layout: post
title: Exposed ORM 복합키 사용
categories: [일상 중 궁금했던]
comments: true
---

## Exposed ORM에서 복합 키(Composite Primary Key) 설정

Exposed ORM에서 기본 키가 2개 이상일 경우 `IntIdTable`이 아닌 `Table`을 사용해야 한다. 
`primaryKey` 속성을 활용하여 복합 키를 설정할 수 있다.

### 예제 코드
```kotlin
object Orders : Table("orders") {
    val orderId = integer("order_id")
    val productId = integer("product_id")
    val quantity = integer("quantity")

    override val primaryKey = PrimaryKey(orderId, productId, name = "PK_Orders")
}
```

이렇게 설정하면 `orderId`와 `productId`가 함께 기본 키가 된다.

### 데이터 삽입 예제
```kotlin
Orders.insert {
    it[orderId] = 1
    it[productId] = 100
    it[quantity] = 2
}
```

### 데이터 조회 예제
```kotlin
val order = Orders.select { (Orders.orderId eq 1) and (Orders.productId eq 100) }.singleOrNull()
```

위 방법을 사용하면 Exposed ORM에서 복합 키를 쉽게 적용할 수 있다.

