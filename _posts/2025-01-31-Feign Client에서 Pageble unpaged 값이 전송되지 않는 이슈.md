---
layout: post
title: Feign Client에서 Pageble unpaged 값이 전송되지 않는 이슈
categories: [일상 중 궁금했던]
comments: true
---

### 문제 원인
- `Pageable.unpaged()`는 페이지 정보가 없는 상태를 의미하며, 내부적으로 빈 객체로 처리됨.
- `Feign Client`는 값이 없거나 `null`인 경우 해당 파라미터를 전송하지 않음.
- 따라서 `unpaged` 상태에서는 `page`, `size`, `sort` 등의 값이 요청에 포함되지 않음.

### 해결 방법
#### 1. Pageable을 개별 파라미터로 전송
```kotlin
@FeignClient("your-api")
interface YourApiClient {

    @RequestMapping("your-api-endpoint")
    fun getItems(
        @RequestParam page: Int = 0,
        @RequestParam size: Int = 10,
        @RequestParam sort: String? = null  // `unpaged`일 경우 null
    ): ResponseEntity<List<Item>>
}
```
- `unpaged()` 상태에서는 `sort`를 `null`로 처리하고, `page`와 `size`는 기본값을 설정하여 전송.

#### 2. Pageable을 PageRequest로 변환 후 전송
```kotlin
@FeignClient("your-api")
interface YourApiClient {

    @RequestMapping("your-api-endpoint")
    fun getItems(
        @RequestParam page: Int = 0,
        @RequestParam size: Int = 10,
        @RequestParam sort: String? = null
    ): ResponseEntity<List<Item>>

    fun getItemsWithPageable(pageable: Pageable): ResponseEntity<List<Item>> {
        val page = pageable.pageNumber
        val size = pageable.pageSize
        val sort = pageable.sort?.toString()
        
        return getItems(page, size, sort)
    }
}
```
- `Pageable.unpaged()` 상태에서도 `page`, `size`, `sort` 값을 명시적으로 변환하여 요청을 보냄.

### 처리 방법
- 나의 경우 모든 값을 보이기 위해 unpaged로 지정하여 전송하려 했으나, 이러한 경우에흔 프론트 동료와 상의하여 최대 페이징 개수를 지정해서 보내는 것이 더 효율적이라고 함.

