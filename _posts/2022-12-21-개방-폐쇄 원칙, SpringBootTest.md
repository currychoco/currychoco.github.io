---
layout: post
title: 개방-폐쇄 원칙, 
categories: [스프링 입문]
comments: true
---

* 개방-폐쇄 원칙(OCP : Open-Closed Principle)
  * 확장에는 열려있고, 수정, 변경에는 닫혀 있음
* 스프링의 DI를 사용하면 기존 코드를 전혀 손대지 않고 설정만으로 구현 클래스 변경이 가능

-------

* @SpringBootTest : 스프링 컨테이너와 테스트를 함께 실행
* @Transactional : 테스트 시작 전에 트랜잭션을 시작하고, 테스트 완료 후에 항상 롤백 -> DB에 데이터가 남지 않으므로 다음 테스트에 영향을 주지 않음
