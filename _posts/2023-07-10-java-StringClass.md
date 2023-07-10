---
layout: post
title: "[JAVA] String에서 +를 하면 안되는 이유 "
date: 2023-07-10 23:51:00 +0900
categories: TIL
---

[https://pingu2017.github.io/algorithm/2023/07/10/beakjoon-9252.html](https://pingu2017.github.io/algorithm/2023/07/10/beakjoon-9252.html)

이 문제를 풀면서 String +연산의 난관에 부딛쳤다.

기존의 다른 언어에서는 문자열을 char형의 배열로 다루지만 자바에서는 문자열을 위한 클래스를 제공한다!

### immutable Class

String클래스에는 문자열을 저장하기 위해서 문자형 배열 참조변수(char[]) value를 인스턴스 변수로 정의해놓고 있다. 인스턴스 생성 시 생성자의 매개변수로 입력받는 문자열은 이 인스턴스변수에 문자형 배열로 저장되는 것이다.

한번 생성된 String인스턴스가 갖고 있는 문자열은 읽어 올 수만 있고 변경할 수는 없다. 따라서 + 연산자를 이용해 문자열을 결합하는 경우 인스턴스 내 문자열이 바뀌는 것이 아니라 새로운 문자열이 담긴 String인스턴스가 생성된다.
