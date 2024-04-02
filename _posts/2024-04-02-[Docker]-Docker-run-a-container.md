---
title: "[Docker] Docker run a container"
date: 2024-04-02 21:00:00 +0900
categories: [Study, Docker]
tags: [Docker]
---

# Docker 실행해보기

<aside>
💡 Docker 공식문서 따라가기

</aside>

![](../../../Desktop/스크린샷 2024-04-02 오후 8.32.58.png)

- 이미지 빌드
```
$ docker build -t welcome-docker-main .
```
![](../../../Desktop/스크린샷 2024-04-02 오후 8.42.58.png)

- 생성된 이미지 확인
```
$ docker image ls
```
![](../../../Desktop/스크린샷 2024-04-02 오후 8.58.06.png)

- docker desktop 실행
![](../../../Desktop/스크린샷 2024-04-02 오후 8.43.35.png)

- ![](../../../Desktop/docker.png)

- 참고
  - https://docs.docker.com/guides/walkthroughs/run-a-container/
