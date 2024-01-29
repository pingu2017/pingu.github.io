---
layout: post
title: "[Docker] 도커 설치 (window)"
date: 2024-01-29 23:14:00 +0900
categories: TIL
---

예에전에는 window pro 가 있어야만 Docker Desktop을 사용할 수 있었다고 한다. pro에서만 Hyper-V를 설치할 수 있기때문인데, Home에선 아직도 Hyper-V 설치가 불가능하지만, 업데이트로 인해 WSL를 지원해주어 Docker Desktop을 사용할 수 있게 되었다.

Windows Subsystem for Linux 2의 줄임말로 윈도우에서 리눅스를 사용할 수 있게 해주는 기능이다.

**wsl 설치**

```
$ wsl --install
```

**wsl 버전 변경**

```
$ wsl --set-default-version 2
```

그 다음엔 [docker.com](https://www.docker.com/)에 접속해 운영체제에 맞는 설치파일을 받아주면 된다!
