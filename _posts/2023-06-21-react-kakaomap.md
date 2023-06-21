---
layout: post
title: "[React] react-kakao-maps-sdk 사용해 지도사용하기"
date: 2023-06-21 00:20:00 +0900
categories: TIL
---

프로젝트를 리팩토링하면서 프론트 코드를 처음부터 싹 다시 만들었다!
이 과정에서 기존에 쓰던 카카오맵도 다시 설정해야했는데, 이번에 내가 역할을 맡았다.

맨 첫번째로 react 프로젝트에 sdk를 설치해주어야 한다.

```
npm i react-kakao-maps-sdk
```

다운받았다면, js에서 `import { CustomOverlayMap, Map, MapMarker } from "react-kakao-maps-sdk";` 를 임포트할 수 있다.

public -> index.html 에 주소 추가

```
<script type="text/javascript" src="//dapi.kakao.com/v2/maps/sdk.js?appkey=발급받은 APP KEY를 넣으시면 됩니다."></script>
```

여기서 KEY는 JavaScript 키를 사용해야 한다.

이제 컴포넌트로 카카오맵을 불러올 수 있다!

<details>
<summary>참조</summary>
<div markdown="1">

https://www.npmjs.com/package/react-kakao-maps-sdk

https://github.com/JaeSeoKim/react-kakao-maps-sdk

</div>
</details>
