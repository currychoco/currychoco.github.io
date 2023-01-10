---
layout: post
title: 실행환경에 따른 properties 분기
categories: [ASSET_MANAGER]
comments: true
---

###### 실행 환경에 따라 바뀌는 것이 고정적일 떄!
- 이럴 경우 profile 분기점을 만들어 해결할 수 있음
![image](https://user-images.githubusercontent.com/107798750/211357480-1b1a8c70-459d-428f-b80f-5ffe7f5fbc4b.png)
- 원래 application.properties 하나만 있었으나, 로컬 서버와 개발 서버에 따라 내용이 바뀌는 변수가 있었기에 application.properties-dev 파일을 생성함
- 해당 파일을 생성하면 프로젝트 실행 시 옵션에 따라 기본 프로파일을 사용할지, 아니면 특정 프로파일을 사용할지 결정할 수 있음
- **--spring.profiles.active=dev** 실행 시 해당 옵션을 붙여 실행시키면 기본 프로파일이 아닌, **dev** 프로파일이 실행됨
- 이러한 방법을 이용하여 로컬, 개발, 운영 서버에서 다르게 적용되어야 할 부분들을 직접 수정할 필요 없이 실행 시 옵션 선택으로 변경할 수 있음


###### 인텔리제이에서 실행 옵션 변경하기
![image](https://user-images.githubusercontent.com/107798750/211358954-4f42a388-aa7b-4615-ba77-a25548f7de6b.png)

![image](https://user-images.githubusercontent.com/107798750/211359125-db03b8d8-23f1-4ddd-8f02-4df18aaf410d.png)

- 상기 과정을 통해 Run/Debug Configuration 을 수정할 수 있으며, 로컬 개발 시에 원하는 profile 을 선택하여 테스트를 실행하는 등의 작업을 할 수 있음



-------


참고
- https://lejewk.github.io/springboot-gradle-spring-profiles-active/
