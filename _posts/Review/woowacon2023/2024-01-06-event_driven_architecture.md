---
layout: single
title:  "[우아콘2023] 배민스토어에 최신 기술 한방에 때려넣기: EDA"
excerpt: "EDA 정리"
header:
  teaser: 
search: false
permalink:
categories: 
  - Review
tags:
  - woowacon
date:  2024-01-06 16:00:00 +0900
last_modified_at: 2024-01-06 16:00:00 +0900
toc: true
toc_sticky: true
toc_label: "Contents"
toc_icon: "cog"
---



# Event Driven Architecture 적용

해당글은 2023 우아콘 아래영상을 보고 인상깊어 일부 정리한 내용입니다.

배민스토어에 최신 기술 한방에 때려넣기: Kotlin, Spring WebFlux, EDA
링크 : [https://youtu.be/pRpryoQphXQ?si=2IPyXY-KpLTLDo2z](https://youtu.be/pRpryoQphXQ?si=2IPyXY-KpLTLDo2z)

## 배민스토어 전시 도메인

배민스토어 전시 도메인 
-> 가게와 상품 노출
-> 검색, 추천, 리뷰, 쿠폰 등 다양한 정보 제공
-> 메인영역, 기획전 등  정보 제공

즉, CRUD 중 Read가 많은 특징이 있다. 많은 정보를 노출하게된다.

다양한 연동 조회를 해오게 되는데 각 장애를 전파하게될 수 도 있다.

EDA(Event Driven Architecture)를 도입하여 안정적이고 빠르게 되도록 결정했다.

이벤트는 발생한 사실 그 자체이고 이벤트라는 것은 변경이 불가능하다
ex) 장바구니에 담는다 상품을 주문한다 상품에 쿠폰을 적용한다 등

## EDA 장단점

EDA는 장점과 단점이 존재한다.
- 장점 :
  - 느슨한 결합 가능 
  - 확장성 좋음
  - 도메인에 특화된 데이터 모델을 편리하게 구성할 수 있다.
- 단점 : 아키텍쳐 구성시 난이도가 높아진다.

카프카 같은 솔루션을 이용하여 EDA 장점을 활용한다.

## 데이터 구성 방식

각 이벤트는 도메인별로 크기가 다르다. 최적화 방식을 고민해야한다.
각 연동 도메인 이벤트에 따라 전시 도메인에 특화되어있는 데이터 모델로 재정의

다양한 데이터들은 가게(Shop) 혹은 셀러(Seller) 중심으로 모이게된다.
- ShopId를 기준으로 데이터를 모아서 조회할 수 있도록 한다.
- RDB 보다는 NoSQL 을 이용을 하여 Key-Value 형태로 저장하면 더 빠르게 접근할 수 있다고 생각했다.

## EDA 도입 이후 겪은 문제

이벤트 연동의 데이터의 양이 이렇게 많은지 몰랐다.
- 전시관련 도메인만 10개 이상이고

도메인 이벤트 개수도 많다
- 상품 데이터를 연동하는데 N천만 건 이상을 수집해야 했다.
- 재고 이벤트는 현재 N억 건 발행되었다.
- 그 외에 연동 데이터들도 존재한다.

모든 도메인에 대한 데이터는 이벤트로 연동할 수 없다.

스키마 변경이 자주 있어 계속 수정된다

연동 에러가 있을경우 알림 설정을 했는데 시스템이 커짐에 따라 알림도 커졌다.

### 데이터 수집을 못한 경우

데이터 수집 딜레이문제 존재

특정 컨슈머들이 정상적으로 동작하지 않아 이벤트 밀리는 문제

데이터 보정에 대한 생각을 많이 하게 된다.

### 문제를 해결하기 위해...

DLT를 적극적으로 활용하는 방법을 고민
- 스스로 복원력을 갖는 시스템을 구상
- 실패 저장소 혹은 이벤트 재발행을 할 수 있도록 구현

서킷 브레이커를 활용 : 동기식으로 연동하는 API에 대해 서킷브레이커 활용

지속적인 도메인 이벤트 최적화 : 데이터 

## EDA 적용을 고민하는 분들에게

새로운 기술을 도입할때 많은 조건들이 필요하다고 생각
- 적절한 개발 기간이 존재하는가
- EDA를 적용하여 많은 이점을 얻을 수 있는가

단순히 새로운 기술, 혹은 도전해 보고 싶은 기술이라면 비즈니스에 적합하지 않을 수 있음
- 기술을 비즈니스에 맞추는 것이 아닌 비즈니스에 기술을 맞추어야 한다.
- 유연한사고를 통하여 올바른 비즈니스 아키텍쳐를 구현해야 한다.

아키텍쳐는 유일하다
- 각 비즈니스별로 최적화된 아키텍쳐를 구현하는 것을 가장 추천

