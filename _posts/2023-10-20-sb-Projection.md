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

<br>

++ projection과 entity조회의 성능차이가 실제하냐는 질문을 받았다. entity는 select all 인데 영속성 컨텍스트에 등록되고, projection은 특정 컬럼만 조회하기때문에 영속성 컨텍스트에 저장되지는 않는다. select \* 쿼리와 select id 쿼리는 는 유의미한 차이가 있다. 특정 컬럼만을 불러오는 것은 전체를 조회하는것보다 성능적인 면에서 우수하다. 하지만 2차캐싱이 되어서 DB에 자주 접근하지않는다면? 이야기가 달라질수도 있겠다. 상황에 맞게 사용하면 될 것이다.

<br>

![select](https://user-images.githubusercontent.com/115390100/277361054-51815905-b339-407d-8061-1ae47abb5edb.png)
