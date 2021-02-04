---
layout: post
title: code-server
categories:
  - web vscdoe
tags:
  - back-end
---

# VSCode 웹버전 설치하기

VSCode를 브라우저에서 클라우드 IDE로 사용하기 위해 서버에 docker로 설치를 한다.

## docker, docker-compose

설치하기 위해서는 우선 docker와 docker-compose를 설치해야 한다.

설치 방법은 docker hub의 링크로 대체 한다.

[docker hub](https://docs.docker.com/compose/install/)



## code-server image

docker로 실행하기 위한 code-server의 이미지를 찾는다.

https://hub.docker.com/r/codercom/code-server



## Docker-compose.yml 작성

```bash
version: '3'

services:
  code-server:
    image: codercom/code-server:latest
    volumes:
      - ${HOME}/.config:/home/coder/.config
      - ${PWD}:/home/coder/project
    environment:
      - DOCKER_USER=${env}:UserName
      - USER=${env}:UserName
    ports:
      - "8080:8080"
      - "3000:3000"
      - "8082:8082"
```

- 환경 변수는 $PATH의 경우 ${PATH} 로 중괄호를 이용하여 객체로 넣는다.
- 모든 값들은 앞에 '-' 를 붙여 같은 배열임을 명시한다.



## Docker-compose 실행

```bash
$ docker-compose up -d
```

[실행옵션](https://nirsa.tistory.com/81)



## 접속

브라우저에서 [서버주소:포트번호]를 입력하여 접속한다.



## 관리자 비밀번호 입력

vscode가 실행되면 관리자 비밀번호를 확인해야 하는데 docker내부의 .config위치를 사용자의 .config 위치로 마운트 해놓았기 때문에 다음으로 password를 확인하면 된다.

```bash
$ cat ~/.config/code-server/config.yaml
```

만약, docker를 여러번 실행하여 기존의 config.yaml이 남아있는 경우 새로운 파일로 제대로 마운트가 되지 않는 경우가 있어  docker 내부로 들어가 패스워드를 읽어 와야 할 수 있다.

이 경우 다음의 명령어로 접속하여 읽어 온다. (내가 자꾸 까먹어서...)

```bash
$ docker exec -it [docker id ] /bin/bash
```





[포트포워딩](https://m.blog.naver.com/PostView.nhn?blogId=alice_k106&logNo=221364560794&proxyReferer=https:%2F%2Fwww.google.com%2F)

외부에서 접속할시 포트 포워딩 (터널링)을 사용하여 접속한다.

```bash
$ ssh -L 8080:localhost:8080 [server_ip] -p [port_num] -l [user_id]
```

- 서버의 localhost:8080 을 server_ip와 port를 이용한 ssh 채널을 통해 내 컴퓨터의 8080 포트에 바인딩 한다.
- 위의 커맨드를 입력한 쉘을 계속 유지해야 터널이 유지 된다.
- 브라우저에서는 localhost:8080을 입력 하면 서버에서 localhost:8080을 입력한 것과 동일한 페이지가 열린다.

