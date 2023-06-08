---
layout: post
title: "[React] click을 안 했는데 onClick이 실행되요"
date: 2023-06-08 22:12:00 +0900
categories: TIL
---

### 문제

```js
  const goHospitalDetail = () => {
    navigation(`/hospitaldetail/1`);
  };
    ...
   onClick={goHospitalDetail()}
```

클릭이벤트로 위의 함수를 실행시키고 싶었는데, 상기한 것 처럼 함수를 넣으면 페이지를 렌더링하면서 함수가 실행되어버린다.

### 원인

react의 onClick은 매개변수를 함수로 받는데 js에서 함수의 이름만 쓰는 것은 함수 자체를 호출하는 것이고, `함수()`를 쓰는 것은 함수를 실행하고 그 return값을 넣는 것이다.

따라서 위와 같이 사용하면 `goHospitalDetail`의 return 값을 얻기 위해 함수를 실행시켜버리는 것이다!

옳은 사용

```js
    onClick={goHospitalDetail()}

    //익명함수를 이용해 감싸주기
  onClick={() => {
        goHospitalDetail();
      }}

```
