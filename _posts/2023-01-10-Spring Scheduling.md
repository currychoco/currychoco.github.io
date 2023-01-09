---
layout: post
title: Spring Scheduling
categories: [ASSET_MANAGER]
comments: true
---

###### 자동으로 시간에 따라 특정 메소드 실행하기

``` java
@Scheduled(cron = "0 0/1 * 1/1 * ?") // 1분에 한 번
	public void test() {
		System.out.println("test1 = " + test1);
	}
```
- cron tab : 유닉스 계열에서 실행 주기를 표현하는 표현식(CronMaker에서 시간에 따른 표현식 받을 수 있음)
  - (cronmaker : http://www.cronmaker.com)
- @Scheduled 어노테이션을 통해 입력한 시간(cron 표현식 지원)마다 실행이 되게 함
- @Scheduled는 빈으로 등록된 클래스 내에서 사용 가능

-------


참고
- https://jeong-pro.tistory.com/186
