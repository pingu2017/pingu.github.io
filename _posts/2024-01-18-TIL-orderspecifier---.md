---
layout: post
title: "[Querydsl] 동적 정렬 : OrderSpecifier "
date: 9999-01-18 23:14:00 +0900
categories: TIL
---

게시판 검색+조회기능을 만들다가
필터링하는 건 where절에서 BooleaExpress를 이용할 수 있는데 다양한 정렬 조건을 넣어야 하는 경우 다수개의 sql을 작성해야하나..? 고민이 되었다. 어떻게 구현할까 고민하다가 OrderSpecifier을 발견!
