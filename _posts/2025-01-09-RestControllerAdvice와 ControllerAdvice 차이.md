---
layout: post
title: RestControllerAdvice와 ControllerAdvice 차이
categories: [일상 중 궁금했던]
comments: true
---

## 1. 개념
- `@ControllerAdvice`와 `@RestControllerAdvice`는 전역적으로 예외를 처리할 때 사용하는 어노테이션임
- 특정 컨트롤러에서 발생하는 예외를 일괄 처리하여 응답 형식을 통일할 수 있음

## 2. 차이점 비교
| 어노테이션 | 설명 | 반환 방식 |
|------------|------------------------------------------------|--------------|
| `@ControllerAdvice` | 일반적인 MVC 컨트롤러(`@Controller`)에서 사용됨 | View(템플릿) 또는 JSON |
| `@RestControllerAdvice` | RESTful API 컨트롤러(`@RestController`)에서 사용됨 | JSON 응답 전용 |

## 3. 언제 사용할까?
- **`@ControllerAdvice`**: View를 반환하는 Spring MVC 프로젝트에서 사용함
- **`@RestControllerAdvice`**: JSON 기반 API 프로젝트에서 사용함 (`@ResponseBody`가 포함된 `@RestController`와 함께 사용됨)

## 4. 예제 코드
### `@ControllerAdvice` 예제 (뷰 반환)
```kotlin
@ControllerAdvice
class GlobalExceptionHandler {
    @ExceptionHandler(Exception::class)
    fun handleException(model: Model, ex: Exception): String {
        model.addAttribute("errorMessage", ex.message ?: "Unknown error")
        return "errorPage"  // errorPage.html을 렌더링함
    }
}
```

### `@RestControllerAdvice` 예제 (JSON 반환)
```kotlin
@RestControllerAdvice
class GlobalExceptionHandler {
    @ExceptionHandler(Exception::class)
    fun handleException(ex: Exception): ResponseEntity<Map<String, Any>> {
        val response = mapOf(
            "success" to false,
            "message" to (ex.message ?: "Unknown error"),
            "status" to HttpStatus.INTERNAL_SERVER_ERROR.value()
        )
        return ResponseEntity.status(HttpStatus.INTERNAL_SERVER_ERROR).body(response)
    }
}
```

## 5. 결론
- **템플릿(View) 기반 프로젝트** → `@ControllerAdvice` 사용
- **REST API 프로젝트** → `@RestControllerAdvice` 사용
- `@RestControllerAdvice`는 `@ControllerAdvice + @ResponseBody`와 동일한 기능을 함


