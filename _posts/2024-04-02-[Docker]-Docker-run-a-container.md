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

<img width="566" alt="스크린샷 2024-04-02 오후 8 32 58" src="https://github.com/dhyun10/dhyun10.github.io/assets/70506979/a6c70bcc-c5f4-423b-9a3a-003973f1b619">

- 이미지 빌드
```
$ docker build -t welcome-docker-main .
```
<img width="566" alt="스크린샷 2024-04-02 오후 8 42 58" src="https://github.com/dhyun10/dhyun10.github.io/assets/70506979/4c347f9f-18c9-471b-81e7-3e7a67009346">

- 생성된 이미지 확인
```
$ docker image ls
```
<img width="562" alt="스크린샷 2024-04-02 오후 8 58 06" src="https://github.com/dhyun10/dhyun10.github.io/assets/70506979/0f74344a-a9b6-44ac-8243-02b7ba6d8c35">

- docker desktop 실행
<img width="1382" alt="스크린샷 2024-04-02 오후 8 43 35" src="https://github.com/dhyun10/dhyun10.github.io/assets/70506979/7f938427-3099-4db4-85d8-a6e7f898697f">

- 컨테이너 실행 완료
<img width="1080" alt="docker" src="https://github.com/dhyun10/dhyun10.github.io/assets/70506979/f35071c0-4e51-46ba-8750-e0de06d31c9a">

- 참고
  - https://docs.docker.com/guides/walkthroughs/run-a-container/
