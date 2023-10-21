---
layout: post
title: "[SpringBoot] Projection in querydsl"
date: 2023-10-20 21:02:19 +0900
categories: TIL
---

Projection이라는 JPA를 이용한 select는 entity를 불러온다. 그래서 대체로 필요없는 컬럼도 불러오게 되고 성능에 영향을 미친다. 하지만 프로젝션은 원하는 컬럼만 조회할 수 있기때문에 성능적으로도, 코드효율적으로도 좋다.

정확한 작동방법 알기

Projection을 사용하는 방법은 setter를 사용하는 .bean, field, 와 생성자를 사용하는 constructor가 있다.

다음과 같이 사용할 수 있다.

```java

JPAQuery<CityFilterDto> content = queryFactory
			.selectDistinct(Projections.constructor(
				CityFilterDto.class,
				city.name,
				country.name,
				countryLanguage.language,
				country.region
			))

```

bean이나 field는 setter를 사용하기때문에 dto 변수의 타입과 이름이 entity와 정확히 일치해야 사용이 가능하다. 그렇지 않으면 null을 반환한다.
constructor는 말 그대로 생성자를 이용하기때문에 dto에 어노테이션이 필요하고, 타입만 일치하면 잘 생성된다.
