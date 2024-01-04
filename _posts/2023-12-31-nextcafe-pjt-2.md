---
layout: post
title: "[NextCafe] 패키지 재설계"
date: 2023-12-31 23:14:00 +0900
categories: project
---

지난 글 [https://pingu2017.github.io/project/2023/12/16/nextcafe-pjt-1.html](https://pingu2017.github.io/project/2023/12/16/nextcafe-pjt-1.html)

일단 개발에 덤벼들었는데 아키텍처가 정립되지 않은 상태로는 개발이 불가능했다! 어떤 class를 어떤 pakage에 넣어야할지 헷갈리는 상태를 해결하기 위해 다시 헥사고날 아키텍처 공부로 돌아갔다ㅜㅜ

헥사고날 아키텍쳐의 요점은 **서로 간섭받지 않는 독립적 모듈 구현**이라고 생각한다. usecase를 사용하고, 헥사곤을 나누어 하나의 큰 프로젝트를 작은 덩어리로 분리하는 것이다.
이 명제를 기준으로 다시 패키지를 설계해보았다.

```
├─main
│  ├─java
│  │  └─com
│  │      └─next
│  │          └─cafe
│  │              ├─board
│  │              │  ├─adapter
│  │              │  │  ├─in
│  │              │  │  └─out
│  │              │  ├─application
│  │              │  │  ├─in
│  │              │  │  ├─out
│  │              │  │  └─usecase
│  │              │  └─domain
│  │              │      ├─entity
│  │              │      └─vo
│  │              ├─common
│  │              │  └─config
│  │              ├─post
│  │              │  ├─adapter
│  │              │  │  ├─in
│  │              │  │  │  └─dto
│  │              │  │  └─out
│  │              │  ├─application
│  │              │  │  ├─in
│  │              │  │  └─out
│  │              │  └─domain
│  │              │      ├─entity
│  │              │      └─vo
│  │              ├─reply
│  │              │  ├─adapter
│  │              │  ├─application
│  │              │  └─domain
│  │              │      ├─entity
│  │              │      └─vo
│  │              └─user
│  │                  ├─adapter
│  │                  ├─application
│  │                  └─domain
│  │                      ├─entity
│  │                      └─vo
│  └─resources
│      ├─sql
│      ├─static
│      └─templates
└─test
    └─java
        └─com
            └─next
                └─cafe
```

기존 헥사고날의 분류와 같이 adapter, application, domain을 두고 in, out를 명시해 흐름을 잘 파악할 수 있도록 했다. adapter-in 의 controller로 요청을 받고 application에서 비즈니스 로직을 구현하고, adpater-out의 repository로 DB를 실행한다.
컨트롤러는 크게 비슷한 사용을 하는 기능끼리 묶어 너무 class가 너무 세분화 되지 않도록 하려고 한다.
개발을 진행하면서 헥사고날 아키텍처의 장점을 체감해 볼 수 있었으면 좋겠다!
