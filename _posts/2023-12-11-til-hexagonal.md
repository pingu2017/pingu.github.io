---
layout: post
title: "[TIL] Hexagonal Architecture (feat.chatGPT) "
date: 2023-12-11 22:47:00 +0900
categories: TIL
---

## Intro

예전 팀 프로젝트에서 잘못된 DB를 골랐다는걸 알게되었지만, 이미 개발이 한창 진행된 후라서 도저히 처음부터 수정할 엄두가 나지 않았다.. 그래서 원하는 기능을 제대로 구현하지 못한 채로 프로젝트를 마무리했던 기억이 있다. 이번 프로젝트에서는 같은 실수를 반복하지 않기 위해 헥사고날 아키텍처를 도입해보도록 하자.
<br>

그동안 나는 아무생각없이 전통적인 아키텍쳐를 사용하고 있었다.

↓나에게 백엔드 서비스를 개발하라하면 내놓는 패키지 구조

```
└─src
    ├─main
       ├─java
          └─com
              └─roller
                  ㅡmain
                      │
                      ├─api
                      │  ├─controller
                      │  ├─request
                      │  ├─response
                      │  └─service
                      ├─config
                      ├─db
                      │  ├─entity
                      │  └─repository
                      └─util

```

헥사고날 아키텍쳐를 공부해 더 멋진 개발자가 되어보자.

### 소프트웨어 아키텍쳐란 무엇인가

건물과 소프트웨어가 아키텍쳐라는 단어를 공유하는 것은 단어가 가진 두 가지의 함의 때문일 것이다.

1. 튼튼하게 잘 설계되어 기능을 수행할 것
2. 쉽게 유지보수 할 수 있을 것

소프트웨어를 개발하다보면 '짓는다'라는 단어가 잘 어울린다는 생각이 들곤한다. DB설계부터 패키지설계, 변수설정 등 하나하나 꼼꼼히 기초를 다져야하기 때문이다. 외적으로 정상적인 작동도 중요하지만 비기능적 요구사항-보안, 유지보수성, 운영가능성 등 보이지않는 내부적 문제를 총괄해 고려해야한다.

### 헥사고날 아키텍쳐란 무엇인가

> 여러분의 애플리케이션을 UI나 데이터베이스 없이 동작하도록 만드십시오. 그러면 애플리케이션에 대해 자동화된 회귀 테스트를 실행할 수 있고, 데이터베이스를 사용할 수 없을 때에도 동작합니다. 그러면 어떤 사용자의 개입없이도 애플리케이션을 함께 연결할 수 있습니다.

이 문장은 헥사고날 아키텍처를 이해하기 위한 토대라고 한다.
헥사고날 아키텍쳐의 주요 개념 중 하나는 비즈니스 코드를 기술코드로부터 분리하는 것이다. 그뿐만 아니라 기술측면이 비즈니스 측면에 의존하는지도 확인해 비즈니스 측면이 비즈니스 목표를 달성하는데 사용되는 기술에 대한 우려 없이도 발전할 수 있게 해야한다. 또한 관련된 비즈니스 코드를 해치지않고 기술 코드를 변경할 수 있어야 한다.
헥사고날 아키텍쳐에서는 이를 위해 3가지의 헥사곤을 만든다.

- 도메인 헥사곤
- 애플리케이션 헥사곤
- 프레임워크 헥사곤

이에 대해 하나씩 알아보도록 하자!

<br>

- 도메인 헥사곤

  - DDD(도메인 주도 개발)의 그 Domain Layer로 기술에 독립적인 POJO로 개발.
  - Utility, Extension, Constants, Entity, VO(Enum 포함), Aggregate를 포함하며 프로젝트 도메인의 비즈니스 룰을 정의
  - POJO로 구현하기 때문에 Spring의 Component, Service annotation 등 비사용
  - 비즈니스 규칙을 위한 Service 개념이 존재할 수 있어야 하지만 static class의 메서드로만 정의
  - 실제로 gradle에 어떤 dependency도 포함하지 않음

<br>

- 애플리케이션 헥사곤

  - Domain의 구성요소를 사용하여 시스템이 가지는 기능/사례(usecase)를 정의한 집합
  - logging, exception, usecase, outputPort(interface) 등을 포함
  - DB가 어떤 것인지, 외부에서 시스템을 가동하기 위한 기술은 무엇인지 아무것도 알 필요가 없다. 알아서도 안된다
  - 팀에 신규 인력이 추가되거나 더 나아가 PO(ProductOwner), PM(ProductManager) 같이 비 개발 인력도 읽을 수 있을만한 쉬운 코드로 작성
  - POJO로 개발하고 다른 영역에서 DI를 제어해야 하지만 "Spring framework을 버리고 다른 DI Framework를 사용할 여지가 있는가?"라는 물음에 "그렇지 않다"라는 결론을 내렸고 각 기능(usecase) 들에 대하여 Service annotation을 사용한 Service로 정의
  - 의존성은 Domain Hexagon에 대해서만 가짐

<br>

- 프레임워크 헥사곤

  - Application hexagon이 소유한 outputPort (interface) 구현체들의 집합
  - Framework Hexagon은 여러 개의 module로 분리, 관리
  - Application Hexagon과 마찬가지로 Spring에 대한 의존성은 가짐
  - 각 Framework Hexagon은 각 기술의 이름을 딴 config class를 포함하며, config class는 그 기술이 사용할 component들을 bean으로 등록할 수 있게 scan 영역을 격리

<br>

헥사고날 아키텍처라고 불리는 이유는 다이어그램으로 볼 때 코어가 중앙에 있고 포트와 어댑터가 측면을 형성하는 육각형과 유사하기 때문이다.

**헥사고날 아키텍쳐의 패키지**

```
 ├─comment
    ├─adapter
    │  ├─in
    │  │  └─web
    │  │  │     Controller.class
    │  │  │
    │  └─out
    │      └─persistence
    │              Entity.class
    │              Repository.class
    │              Mapper.class
    │              PersistenceAdapter.class
    ├─application
    │  ├─port
    │  │  ├─in
    │  │  │  │      UseCase.class
    │  │  │  └─command
    │  │  │         Command.class
    │  │  └─out
    │  │  │         Port.class
    │  │  │
    │  └─service
    │               Service.class
    │
    └─domain
```

### 헥사고날 아키텍처의 장점

- 테스트 가능성: 관심사를 분리하면 외부 종속성을 포함하지 않고 핵심 비즈니스 논리에 대한 단위 테스트를 더 쉽게 작성할 수 있다.

- 유연성: 핵심 로직이 외부 구성 요소와 분리되므로 시스템이 더욱 유연해지고 변화에 적응할 수 있다.

- 유지 관리 용이성: 모듈식 설계를 촉진하여 시스템 유지 관리 및 확장을 더 쉽게 만든다.

- 이식성: 핵심 논리를 변경하지 않고 어댑터를 변경하여 애플리케이션을 다양한 환경에 쉽게 적용할 수 있다.

헥사고날 아키텍처는 확장 가능하고 유지 관리가 가능한 시스템을 구축하는 데 특히 유용하며 종속성 역전(Dependency Inversion) 및 제어 역전(Inversion of Control)과 같은 원칙에 잘 맞습니다.

<br>

헥사고날 아키텍처의 장점은 포트와 어댑터의 관계에서부터 시작한다.

**포트:**

포트는 일련의 작업을 정의하는 인터페이스 또는 추상 클래스이다.
포트는 애플리케이션 코어(비즈니스 로직)와 외부 세계 사이의 경계를 나타낸다.
이는 애플리케이션 코어가 외부 종속성(예: 데이터베이스, 사용자 인터페이스, 서비스 등)과 상호 작용하는 방법을 정의한다.
포트는 애플리케이션 헥사곤에 정의되어 있지만 구현되지는 않는다.

**어댑터:**

어댑터는 포트의 구현이다.
어댑터는 애플리케이션 코어와 외부 종속성 간의 브리지 역할을 한다.
포트에 의해 정의된 인터페이스 또는 추상 클래스를 구현한다.
어댑터는 애플리케이션 코어의 호출을 외부 종속성이 이해할 수 있는 작업으로 변환하거나 그 반대로 변환하는 역할을 담당한다.
어댑터는 시스템의 외부 레이어에 있다.

포트와 어댑터가 분리되어있는, 즉 인터페이스로 인한 의존역전을 실행한 코드에서는 내부 구현이 다른 레이아웃에 영향을 미치지 않기 때문에 모듈화가 가능하다.

---

<details>
<summary>참고자료</summary>
<div markdown="1">

https://techblog.woowahan.com/12720/

https://youtu.be/saxHxoUeeSw?si=B3c5WKUOAESZaC0G

https://youtu.be/dJ5C4qRqAgA?si=7Ffe3M1TYWaGoXwu

</div>
</details>

헥사고날 아키텍처를 이해하기 위해 정말 많은 자료를 찾아본 것 같다. 아직도 완벽히 이해했다고 말할 수는 없지만 새로운 이슈를 만날 준비가 되었다고 생각한다!
