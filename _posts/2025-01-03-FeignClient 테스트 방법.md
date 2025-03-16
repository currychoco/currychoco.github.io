---
layout: post
title: FeignClient 테스트 방법
categories: [일상 중 궁금했던]
comments: true
---

### FeignClient를 사용해서 정상적으로 값이 나오고 있는지 확인하고 싶지만, 외부 API가 아직 완성되지 않았을 때 테스트 방법

# Spring Boot에서 FeignClient 테스트 시 외부 API 호출 없이 값 반환하기

FeignClient를 사용할 때, 테스트 중 실제 외부 API와 통신하지 않고 값을 반환하려면 다음 방법을 사용하면 됨.

## 1. Fallback을 이용한 대체 응답 반환

Feign Client에 fallback 클래스를 지정하면 테스트 중 원하는 값을 반환할 수 있음.

```java
@FeignClient(name = "externalApiClient", url = "http://example.com", fallback = ExternalApiFallback.class)
public interface ExternalApiClient {
    @GetMapping("/api/data")
    String getData();
}
```

```java
@Component
public class ExternalApiFallback implements ExternalApiClient {
    @Override
    public String getData() {
        return "Test data"; // 테스트 중 반환할 값
    }
}
```

## 2. Spring Context에서 Mocking 사용

`@MockBean`을 활용하여 Feign Client를 목(Mock) 객체로 대체할 수 있음.

```java
@SpringBootTest
public class ExternalApiClientTest {

    @MockBean
    private ExternalApiClient externalApiClient;

    @Autowired
    private SomeService someService;

    @Test
    public void testFeignClient() {
        // Mocking Feign Client의 동작
        Mockito.when(externalApiClient.getData()).thenReturn("Mock data");

        String result = someService.getExternalData();
        Assertions.assertEquals("Mock data", result);
    }
}
```

## 3. HTTP 메서드 명시 (오류 해결)

Feign Client에서 HTTP 메서드를 지정하지 않으면 오류가 발생할 수 있으므로 반드시 명시해야 함.

```java
@FeignClient(name = "externalApiClient", url = "http://example.com")
public interface ExternalApiClient {
    @GetMapping("/api/data") // HTTP 메서드 명시
    String getData();
}
```

## 결론
- **Fallback을 사용하면 실제 API 호출 없이 기본 응답을 반환할 수 있음.**
- **테스트 코드에서는 `@MockBean`을 사용하여 Feign Client를 Mocking할 수 있음.**
- **Feign Client는 HTTP 메서드를 반드시 명시해야 오류를 방지할 수 있음.**
