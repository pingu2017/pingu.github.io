---
layout: post
title: "[NextCafe] 패키지 설계 "
date: 2023-12-16 18:54:00 +0900
categories: project
---

헥사고날 아키텍쳐를 적용해 패키지 설계를 해보았다.

### 패키지 구조

```
├─main
│  ├─java
│  │  └─com
│  │      └─next
│  │          └─cafe
│  │              ├─common
│  │              │  └─config
│  │              ├─post
│  │              │  ├─adapter
│  │              │  │  ├─repository
│  │              │  │  └─service
│  │              │  ├─api
│  │              │  │  ├─controller
│  │              │  │  │  └─dto
│  │              │  │  └─exception
│  │              │  ├─application
│  │              │  │  ├─outputport
│  │              │  │  └─usecase
│  │              │  └─domain
│  │              │      ├─entity
│  │              │      └─vo
│  │              └─reply
│  └─resources
│      ├─static
│      └─templates
└─test
    └─java
        └─com
            └─next
                └─cafe
```

<br>

- domain : 프로젝트 도메인의 비즈니스 룰을 정의
- application : 서비스의 기능, usecase, interface
- adapter : application의 구현체
- api : web service의 adapter
- common : configuration 등 공통코드

 <br>

domain -> application -> adapter -> api로 진행되도록 구성하였다.
패키지는 도메인별로 구분해 나중에 기능이 많아졌을 때 복잡성을 줄이도록 했다. 그리고 유즈케이스 기반의 구현을 하기로 한 만큼 도메인이 메인이 되어야 할 것이라고 생각했다.
사실 제대로 짠 패키지인지는 모르겠다....! 개발을 진행해보면서 셀프 피드백을 해보도록 하자.
