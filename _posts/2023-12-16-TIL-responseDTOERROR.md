---
layout: post
title: "[trouble shooting] Resolved [org.springframework.web. HttpMediaTypeNotAcceptableException: No acceptable representation "
date: 2023-12-26 23:14:00 +0900
categories: TIL
---

insert 쿼리가 잘 날아갔는데 swagger에서 406에러가 떴다.

![image](https://github.com/pingu2017/comment/assets/115390100/89d3a09c-d419-42f2-9eff-dab1c4d3f7df)

```
Hibernate: insert into cafe_post (board_id,content,created_date,modified_date,post_like,title,user_id,view_count) values (?,?,?,?,?,?,?,?)
2023-12-26T23:11:42.058+09:00  WARN 4120 --- [nio-8080-exec-4] .w.s.m.s.DefaultHandlerExceptionResolver : Resolved [org.springframework.web.HttpMediaTypeNotAcceptableException: No acceptable representation]
```

처음보는 에러라서 검색해보니까 responseDTO 에서 getter가 없으면 생기는 오류라고 한다. DB를 확인해보니까 insert는 정상적으로 되었는데, 출력이 되지 않은 것이다. DTO자체를 반환할거라고 생각해서 객체를 전해주기만 하면 될 줄 알았는데. 생각해보니까 JSON으로 변환해주는 과정에서 getter가 필요하겠구나. 왠지 swagger api에 response 부분이 빈 객체 {}로 표시되어서 왜그런가 했다ㅋㅋ
무심히 붙이던 @Getter의 중요성!
