---
layout: post
title: "[TIL] Websocket 맛보기 "
date: 2024-01-06 23:47:00 +0900
categories: TIL
---

실무에서 웹소켓을 쓸 일이 생겨서 작은 채팅을 하나 구현해보기로 했다.

### WebSocket이란 무엇인가

웹 소켓은 양방향 통신이 가능하다는 장점때문에 실시간 서비스에서 주로 사용한다. HTTP는 클라이언트가 서버에 요청을 보내고, 서버가 응답하는 단방향 통신이다. WebSocket은 연결을 유지하고 있기 때문에 클라이언트와 서버가 실시간으로 데이터를 주고받는 양방향통신이 가능하다. 처음에 Handshake를 한 번 진행하면 그 다음부터는 양방향 통신이 가능하다. HTTP가 응답을 주고받을때 마다 3 way handshake를 해야하는 것에 대조된다.

WebSocket의 핸드쉐이크 과정은 다음과 같다:

1. 클라이언트가 서버에 WebSocket 연결을 요청하는 HTTP 업그레이드 요청을 보낸다. 이때 사용되는 HTTP 메서드는 GET이고, Upgrade헤더, Connection 헤더, Sec-WebSocket-Key 헤더, Sec-WebSocket-Version 헤더가 포함된다.
2. 서버는 이 업그레이드 요청을 수락하고, HTTP 프로토콜을 WebSocket 프로토콜로 전환한다.
3. 서버는 특정 헤더와 함께 클라이언트에 응답하여 핸드쉐이크를 완료한다.
4. 이후부터는 클라이언트와 서버 간의 양방향 통신이 WebSocket을 통해 이루어진다.

**헤더 예시**

```
GET /chat HTTP/1.1
Host: example.com
Upgrade: websocket
Connection: Upgrade
Sec-WebSocket-Key: dGhlIHNhbXBsZSBub25jZQ==
Sec-WebSocket-Version: 13
```

또한 WebSocket은 Frame 형식을 취하는데 메시지를 작은 프레임으로 나누어 전송하여 전체 메시지를 손실 없이 전달할 수 있게 한다. 각 프레임은 헤더와 payload로 구성된다. WebSocket은 메시지를 작은 프레임으로 분할하여 전송할 수 있으며, 이러한 프레임들이 결합되어 전체 메시지를 형성한다. 프레임은 텍스트 프레임과 이진 프레임이 있다.

### 구현하기

개발 레포지토리 보기 -> [https://github.com/pingu2017/exercise/tree/websocket](https://github.com/pingu2017/exercise/tree/websocket)

gradle에 추가

```java
implementation 'org.springframework.boot:spring-boot-starter-websocket'
```

Configuration 생성

```java
@Configuration
@EnableWebSocketMessageBroker
public class WebSocketConfig implements WebSocketMessageBrokerConfigurer {

	@Override
	public void configureMessageBroker(MessageBrokerRegistry config) {
		config.enableSimpleBroker("/topic");
		config.setApplicationDestinationPrefixes("/app");
	}
	@Override
	public void registerStompEndpoints(StompEndpointRegistry registry) {
		registry.addEndpoint("/chat").withSockJS();
	}
}
```

Controller에 추가

```java
	@MessageMapping("chat/message")
	@SendTo("/topic/public")
	public ChatMessage sendMessage(ChatMessage chatMessage){
		return chatMessage;
	}
```

### 결과 화면

![image](https://github.com/pingu2017/comment/assets/115390100/2b2814c0-526d-4089-9525-cff658c15f24)

<br>

이런 로그도 확인할 수 있다.

```
2024-01-06T23:32:38.451+09:00  INFO 14356 --- [essageBroker-12] o.s.w.s.c.WebSocketMessageBrokerStats    : WebSocketSession[1 current WS(1)-HttpStream(0)-HttpPoll(0), 1 total, 0 closed abnormally (0 connect failure, 0 send limit, 0 transport error)], stompSubProtocol[processed CONNECT(1)-CONNECTED(1)-DISCONNECT(0)], stompBrokerRelay[null], inboundChannel[pool size = 9, active threads = 0, queued tasks = 0, completed tasks = 9], outboundChannel[pool size = 2, active threads = 0, queued tasks = 0, completed tasks = 2], sockJsScheduler[pool size = 16, active threads = 1, queued tasks = 2, completed tasks = 13]
```
