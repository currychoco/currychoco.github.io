---
layout: post
title: lombok의 @RequiredArgsConstructor
categories: [spring]
comments: true
---

##### @RequiredArgsConstructor

- 해당 어노테이션을 사용할 경우 final로 선언된 멤버변수가 매개변수로 지정된 생성자를 자동으로 생성해준다.

``` java

// 어노테이션이 없을 경우
public class TestService {

    private final TestRepository testRepository;

     public TestService(TestRepository testRepository) {
        this.testRepository = testRepository;
    }
}

```

``` java

// 어노테이션이 있을 경우
@RequiredArgsConstructor
public class TestService {
    
    private final TestRepository testRepository;

}

```

- 첫 번째 코드를 보면 의존주입을 받기 위해 TestRepository를 매개변수로 받는 생성자를 생성한 것을 확인할 수 있다.
- 두 번째 코드를 보면 '@RequiredArgsConstructor' 어노테이션이 추가되어 있고 생성자가 없는 것을 확인할 수 있다. l\


  
