---
layout: post
title: "[SpringBoot] Interface 기반 Projection"
date: 2023-10-28 12:02:19 +0900
categories: TIL
---

[관련 repository](https://github.com/pingu2017/exercise/blob/querydsl/exercise/src/main/java/com/pingu/exercise/api/CityProjectionDto.java)

### 사건의 발단 :

실무에서 테이블 5개를 조인해 여러가지 정보를 모아 보여줘야하는 일이 생겼다. 하나하나 findBy로 찾은다음 service단에서 합쳐줘도 되지만, 한 번의 쿼리로 해결하면 멋진 개발자 같아 보인다. 또한 앞선 글에서 보았듯 entity를 부르는 것보다 성능적인 면이 크다. 조인관계가 복잡해 querydsl로는 마음대로 튜닝이 어렵기 때문에 native query를 사용하기로 결정했다.

### 서: Projection이란

쿼리 하나로 다양한 컬럼 값을 select하고, 이것을 넘겨주기 위해서는 DTO가 필요하다. 하지만 repository에서는 entity만 받는데.. 이것을 해결해 줄 수 있는 것이 Projection이다. [관련글 : projection in querydsl](https://pingu2017.github.io/til/2023/10/20/sb-Projection.html)
Projection은 영어 의미 그대로 값들을 투영해 담아준다. querydsl에서는 class형식으로 사용할 수 있다. 하지만 JPQL이나 SQL로 직접 쿼리를 작성하고싶다면 interface를 사용해 return 타입을 설정해야 한다.

### Interface DTO 설정하기

```java
package com.pingu.exercise.api;

import com.fasterxml.jackson.annotation.JsonGetter;

import io.swagger.v3.oas.annotations.media.Schema;

public interface CityProjectionDto {

	@JsonGetter("도시이름")
	@Schema(description = "도시의 이름입니다")
    String getName();

    @JsonGetter("국가 코드")
	String getCode();

    String getRegion();
}

```

get(컬럼이름) 식의 함수로 이름을 지정해주어야 한다. 각 entity개체에 대해 '인터페이스의 프록시 인스턴스'를 만들고, 프록시에 대한 호출은 해당 개체(getName()) 으로 전달되기 때문이다. 쉼게 말하면 entity에서 get해오는 것이다. 이름이 맞지 않으면 찾아갈 수 없다!

> 위의 설명과 예시는 `Closed Projections` 이라고도 한다. 그 반대의 `Open Projections`가 있다. @Value 를 사용해 이름이 일치하지 않아도 값을 사용할 수 있다.

<br>

DB의 컬럼값은 프론트친화적인 작명은 아닐 수 있다. 그래서 @JsonGetter를 사용하면 Json의 키값을 지정해 줄 수 있다.
null값에 대한 처리도 삼항연산자 등을 이용해 interface 내에서 해 줄수 있다.

### 결 :

다음과 같은 json을 얻을 수 있다!
![사진](https://user-images.githubusercontent.com/115390100/278793923-c718dc86-e074-468a-bfb2-f8fbda335f95.png)
