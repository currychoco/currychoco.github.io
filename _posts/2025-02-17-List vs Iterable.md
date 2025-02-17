---
layout: post
title: List vs Iterable
categories: [일상 중 궁금했던]
comments: true
---

# Kotlin에서 Iterable을 왜 사용하는가?

## 1. 지연 연산 가능
`Iterable`은 컬렉션을 한 번에 모두 로드하지 않고, 필요할 때 하나씩 요소를 제공할 수 있다.
- `List`로 변환하면 즉시 모든 요소가 메모리에 로드되지만, `Iterable`을 사용하면 필요할 때 하나씩 변환할 수 있다.
- 특히 대량의 데이터 처리 시 성능과 메모리 사용량을 줄일 수 있다.

```kotlin
fun Iterable<ResultRow>.toBrandList(): Iterable<AutoBrandDto> {
    return this.map { row -> AutoBrandDto(row["name"], row["country"]) } // 지연 연산 가능
}
```
- 위 코드에서 `toBrandList()`가 호출될 때 변환이 즉시 수행되는 것이 아니라, 실제로 순회할 때 변환이 수행된다.

## 2. 다양한 컬렉션과 호환 가능
`Iterable`을 반환하면 `List`, `Set`, `Sequence` 등 다양한 컬렉션 타입과 쉽게 호환된다.

```kotlin
val brandList = resultRows.toBrandList().toList()   // 리스트로 변환
val brandSet = resultRows.toBrandList().toSet()     // 집합으로 변환
val brandSeq = resultRows.toBrandList().asSequence() // 시퀀스로 변환 (추가적인 지연 연산 가능)
```
- `List`로 반환하면 무조건 `List`로 고정되므로 유연성이 줄어든다.
- `Iterable`을 반환하면 호출하는 쪽에서 원하는 형태로 변환하여 사용할 수 있다.

## 3. 불필요한 리스트 생성 방지
`Iterable`을 반환하면 특정 경우에 리스트를 굳이 생성하지 않아도 되어, 불필요한 객체 할당을 줄일 수 있다.

```kotlin
for (brand in resultRows.toBrandList()) { // Iterable을 직접 사용
    println(brand.name)
}
```
- `List`로 반환하면 내부적으로 리스트를 생성해야 하므로 불필요한 메모리 할당이 발생할 수 있다.
- `Iterable`을 그대로 사용할 수 있는 경우, 메모리 사용이 최적화된다.

