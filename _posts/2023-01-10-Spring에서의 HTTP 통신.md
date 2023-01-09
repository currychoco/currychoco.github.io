---
layout: post
title: Spring에서의 HTTP 통신
categories: [ASSET_MANAGER]
comments: true
---

백엔드에서 다른 서버로 Server to Server HTTP 통신을 진행해야 하는 경우가 있다.
Spring 환경에서는 여러 방법을 제공하지만 가장 많이 쓰이는 RestTemplate 라이브러리를 예제를 중심으로 알아보자.

###### 내가 만든 api 다른 서버에서 가져와서 써보기

``` java
@Test
void restTemplate_테스트() {
  final String url = "http://localhost:8080" + "/test-api";
  HttpHeaders headers = new HttpHeaders();
  headers.set("Authorization", key);

  HttpEntity request = new HttpEntity(headers);

  ResponseEntity<List<TestDto>> response = new RestTemplate().exchange(
      url,
      HttpMethod.GET,
      request,
      new ParameterizedTypeReference<>() {}
  );

  System.out.println(response.getBody());
}
```
- HTTP header 부분에 key정보를 추가하기 위해 HttpHeaders 객체를 생성하고 set 메서드로 헤더 데이터 추가
- ResponseEntity는 사용자의 HttpRequest에 대한 응답 데이터를 포함하는 클래스임. 따라서 HttpStatus, HttpHeaders, HttpBody를 포함함
  - ResponseEntity로 감싸서 api를 통한 응답결과를 저장
- RestTemplate는 Spring에서 지원하는 객체로 간편하게 Rest 방식 API를 호출할 수 있는 Spring 내장 클래스
  - 해당 클래스의 메소드인 exchange()를 통해 api 주소, 메서드 방식, 헤더부분에 추가하고 싶은 내용을 저장한 request, 반환받을 형식을 담아 HTTP header로 보냄
  - 요청을 통해 처리된 응답은 response에 저장됨


------

참고
- https://blog.naver.com/hj_kim97/222295259904
