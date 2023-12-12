---
layout: post
title: "[TIL] 그런 REST API로 괜찮은가"
date: 2023-12-12 16:47:00 +0900
categories: TIL
---

[https://youtu.be/RP_f5dMoHFc?si=x8M0kqRLe6kz6sMs](https://youtu.be/RP_f5dMoHFc?si=x8M0kqRLe6kz6sMs)

_위의 영상을 보고 작성된 글입니다._

<br>

rest를 구성하는 스타일

- client-server
- stateless
- cahce
- uniform interface
  - identification of resources
  - manipulation of resources through representations
  - self-descriptive messages : 서버나 클라이언트가 변경되더라도 오고가는 메세지는 언제나 self-descriptive하므로 언제나 해석이 가능하다.
  - hypermedia as the engine of application state(HATEOAS)
- layered system
- code-on-demand (optional)

rest 는 아키텍처이므로 '제약조건'을 모두 지켜야만 rest를 '따른다'고 할 수 있다.
제약조건을 잘 지키지 않는 rest API가 많다!! -> 특히 **uniform interface, HATEOAS**

상호운용성(interoperability)는 중요하다!

- Referer 오타지만 안 고침
- charset 잘못 지은 이름이지만 안고침(encoding 이어야 맞음)
- HTTP 상태 코드 416 포기함
- HTTP/0.9 아직도 지원함(크롬, 파이어폭스)

상호운용성을 고려한 업데이트 덕분에 웹은 서버와 클라이언트의 독립적인 발전이 가능하다.

---

항상 가슴이 시키는대로 api를 지었는데, 정확히 공부해보니까 rest 가 의도했던 장점들이 좋아보인다. 여러가지 API를 설계할 수 있도록 공부해보고 활용해보고 싶다.
