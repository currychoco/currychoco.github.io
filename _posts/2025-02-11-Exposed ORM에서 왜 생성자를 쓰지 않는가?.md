---
layout: post
title: ---
layout: post
title: Stored XSS 방어 방법
categories: [일상 중 궁금했던]
comments: true
---
categories: [일상 중 궁금했던]
comments: true
---

## object와 IntIdTable의 관계
- `object` 키워드를 사용하면 **싱글턴 객체**가 생성됨 → 애플리케이션 실행 중 하나만 존재
- `IntIdTable`은 기본 생성자를 가지고 있지만, `object`가 이를 상속하면 **생성자는 단 한 번만 호출됨**

## 동작 과정
1. **`object Users` 선언 시점**  
   - `object`이므로 Kotlin이 싱글턴 객체를 생성할 준비를 함  
   - `IntIdTable`을 상속받았으므로 부모 클래스의 생성자가 실행됨  
   
2. **부모 클래스(`IntIdTable`)의 생성자 호출**  
   - `id` 필드를 만들고, `primaryKey` 설정 등 초기화 작업 수행  
   - 이 과정은 **오직 한 번만 실행됨**

3. **생성된 `Users` 객체를 싱글턴으로 유지**  
   - 이후 `Users` 객체를 참조하면 기존 객체를 반환  
   - 추가적인 객체 생성은 불가능

## 정리
- `object`이므로 싱글턴이 보장됨  
- `IntIdTable`의 생성자는 **`object`가 생성될 때 한 번만 실행**됨  
- `Users` 객체가 생성된 이후에는 **동일한 인스턴스를 계속 사용**  
- **부모 클래스의 생성자가 중복 호출되지 않으며, `Users` 객체도 한 번만 생성됨**

