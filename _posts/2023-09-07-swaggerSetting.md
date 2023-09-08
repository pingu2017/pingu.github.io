---
layout: post
title: "[SpringBoot] Swagger 설정"
date: 2023-09-08 21:02:17 +0900
categories: TIL
---

API문서를 손쉽게 만들어주는 착한 친구 swagger
postman도 쓰긴 하지만 swagger의 편리함을 따라올자는 없다.. 둘다 쓰는것이 최고 편하긴하다. postman은 입력값 저장도해주고 따로 configration할것도 없고 json으로 export도 해준다! 하지만 개발할땐 swagger를 쓰는게 편리하다.

스프링부트 3에서는 swaggerfox가 지원되지않으니 주의할것.

pom.xml에 추가

```
<dependency>
    <groupId>org.springdoc</groupId>
    <artifactId>springdoc-openapi-starter-webmvc-ui</artifactId>
    <version>2.2.0</version>
</dependency>
```

SwaggerConfig.java

```java
import org.springdoc.core.models.GroupedOpenApi;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

import io.swagger.v3.oas.models.Components;
import io.swagger.v3.oas.models.OpenAPI;
import io.swagger.v3.oas.models.info.Info;

@Configuration
public class SwaggerConfig implements WebMvcConfigurer {

	@Bean
	public GroupedOpenApi api(){
		return GroupedOpenApi.builder()
			.group("이름")
			.pathsToMatch("/api/v1/**")
			.build();
	}
	@Bean
	public OpenAPI getOpenApi() {

		return new OpenAPI().components(new Components())
			.info(info());

	}

	private Info info(){
		return new Info()
			.title("이름")
			.description("설명")
			.version("1.0.0");
	}
}

```
