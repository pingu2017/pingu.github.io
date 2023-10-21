---
layout: post
title: "[SpringBoot] Unknown column 'c1_0.country_code' in 'field list'"
date: 2023-10-19 21:02:19 +0900
categories: TIL
---

Unknown column 'c1_0.country_code' in 'field list'

어디서 본듯한 익숙한 오류.. JPA는 Entity를 기반으로 DB 스키마를 만들 때 단어 사이에 \_ 언더바를 넣는다. 나는 분명

```
	@Column(name = "CountryCode")
	private String Countrycode;
```

이렇게 컬럼명까지 지정해줬는데도 불구하고 country_code로 찾으려한다! 그래서 이것을 강제로 내가 적은데로 찾으라고 하는 설정이 필요하다.
다음과 같이 추가하면 바로 해결.

```
#application.properties

spring.jpa.hibernate.naming.physical-strategy=org.hibernate.boot.model.naming.PhysicalNamingStrategyStandardImpl
```

예전에도 똑같은 문제를 겪었었는데 까먹고있었다ㅎㅎ
