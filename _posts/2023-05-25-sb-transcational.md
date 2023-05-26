---
layout: post
title: "[Springboot] @Transcational @Modifying"
date: 2023-05-25 23:47:00 +0900
categories: TIL
---

repository에서 update나 delete를 할 때 어노테이션이 반드시 필요하다

### repository

```java
    @Modifying
	@Query(value = "Update MarketEntity SET title = :title, content = :content WHERE id = :id")
	void updatePost(@Param("id") long id, @Param("title") String title, @Param("content") String content);
```

### controller

```java
    @Transactional
	@PutMapping("/post/update")
	public ResponseEntity updatePost(@RequestBody UpdatePost updatePost){
		ResponseDto responseDto=marketService.updatePost(updatePost);
		return  ResponseEntity.status(HttpStatus.OK).body(responseDto);
	}
```

`@Transactional`은 트랜잭션의 특징을 가질 수 있게 해주는 친구. 트랜잭션은 데이터베이스 작업의 단위인데 원자성, 일관성, 영속성, 고립성을 가진다. update나 delete에서 작업을 처리하던 중 오류가 발생하면 모든 작업을 rollback 할 수 있다. @Transactional이 붙은 메서드는 메서드가 포함하고 있는 작업 중에 하나라도 실패할 경우 전체 작업을 취소한다.

`@Modifying`은 벌크연산에 이용된다. 다량의 연산을 하나의 쿼리로 처리해 속도를 높여주는 것이 JPA의 장점이다. 하지만 수정한 데이터가 1차캐시에만 적용되고 DB에 적용되지 않는다면 데이터동기화에 문제가 있을 수 있다. 따라서 clearAutomatically 속성을 사용해 @Modifying 어노테이션이 붙은 쿼리 메서드 실행 직후, 영속성 컨텍스트를 clear할 것인지 지정할 수 있다. Persistent 상태에서는 하이버네이트가 한 트랜잭션 내에서 불필요한 쿼리를 줄여주는 중요한 역할을 하는데요. clear하지 않으면 1차캐시가 수정 전 값을 가지고 있다 따라서 수정 후 1차캐시를 지워야 DB에 쿼리를 날려 정확한 값을 불러올 수 있다.
