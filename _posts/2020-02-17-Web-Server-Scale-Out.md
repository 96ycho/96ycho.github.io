---
layout: post
title: Web Server Scale Out
description: >
  2/17 웹 서버 스케일 아웃
tags: [study]
comments: true
---

# 2/17 웹 서버가 두 대로 늘어나면?

## 한대의 서버

### 문제점

#### 가용성



## 세 대의 서버

Server : 1 DB + 2 Web

### Load Balancer

분배와 장애 시 관리

- L4 :  가장 흔한 로드밸런서로 TCP레벨의 port까지

- L7 :  Http까지. 특정 URL을 호출한다. 

  ​		일부러 Fail을 낼 수 있기 때문에 배포 전에 client를 받지 않을 수 있다(사용자 임팩트가 없음)

- DNS Round-Robin 사용 : 단, DNS는 캐싱을 기본으로 한다. DNS변경 시 모두 재 로드 해야하는 문제.

**세션 공유** : 로그인 정보등을 공유 해야한다. 쿠키를 이용할 수도 있음(단, 데이터에 제한)

​					웹 서버간 컨텐츠(세션 등) 공유 필요 -> DB, Redis, NAS/NFS

### 문제점

두 서버가 <u>50%이상의 가용</u>을 하면 안된다.

한 서버가 죽었을 경우 문제 발생.

### L4

<u>DSR(Direct Server Return) 모드</u>

L4가 양방향 프락시라면 모든 웹 서버의 트래픽을 받아야한다.

따라서, client->server로의 트래픽은 L4를 거치지만, server->client로는 L4를 거치지 않고 direct로.



## 과제

#### Redis(개인)

redis-test 10000

Key = "test" Value = "value"로 설정

자바 콘솔 프로그램. 캐시 서버인 redis서버에 접근하여 값을 set하고, get하는 연산을 인자로 받은 10000번 수행하여 걸린 시간을 출력하는 프로그램.

#### 세션관리

- DB로 세션관리 구현
- Redis 세션관리 구현 - Web Server에 띄우기.

Session객체를 사용하지 않고, custom Session 객체를 생성(jedis).

WAS구동 시점에 옵션이나 Spring 설정 파일에서 어떤 구현을 쓸 것인지 선택할 수 있도록 구현.

#### 파일 관리

- TOAST Cloud의 ObjectStorage 사용

- openstack4j SDK 사용

둘 중 선택하여 사용.

#### 로드밸런싱/고가용성

L4, L7 헬스체크 적용

배포 서비스 TOAST Cloud Deploy 적용

production용 빌드를 만들어서 TOAST Cloud에 업로드하고 선택하여 배포. 

DB, Web Server 등 배포 동안 사용자 임팩트가 있으면 안된다. 



#### 교육 내용 출처: NHN 기술교육 - 2/17 웹 서버 스케일 아웃 (이경환 수석)

