---
layout: single
title:  "[자바 ORM 표준 JPA 프로그래밍 - 기본편] 2-1강"
excerpt: "JPA 시작하기 - 프로젝트 생성"
header:
  teaser: 
search: False
permalink:
categories: 
  - BackEnd
tags:
  - java
  - backend
  - study
date:  2021-10-18 23:50:00 +0800
last_modified_at: 2021-10-18 23:50:00 +0800
toc: true
toc_sticky: true
toc_label: "Contents"
toc_icon: "cog"
---
---

https://inf.run/2zDo 강의를 수강하고 작성하는 게시물입니다.

---

# 1. 환경설정

## 1.1. H2 데이터베이스 설치와 실행

http://www.h2database.com 

위의 사이트에서 다운로드 버튼을 누르고 플랫폼에 맞는것을 누른다.

(버전을 몇 받았는지 기억한다.)

## 1.2. 프로젝트 생성

## 1.2.1. 자바

자바 8이상 권장 (필자는 17버전 사용)

## 1.2.2. 메이븐 설정

- groupId: jpa-basic
- artifactId : ex1-hello-jpa
- version : 1.0.0

제일 위의 디렉토리에 있는 pom.xml 을 열고 아래 내용을 추가한다.

```xml
<!-- pom.xml -->

<dependencies>
    <!-- JPA 하이버네이트 -->
    <dependency>
        <groupId>org.hibernate</groupId>
        <artifactId>hibernate-entitymanager</artifactId>
        <version>5.3.10.Final</version>
    </dependency>
    <!-- H2 데이터베이스 -->
    <dependency>
        <groupId>com.h2database</groupId>
        <artifactId>h2</artifactId>
        <version>1.4.199</version>
    </dependency>
</dependencies>

```

위 내용중에서 본인과 맞는 version을 찾는 방법은(spring과 같이 사용해야하므로 spring 과 맞아야한다.)

https://spring.io 에 접속

PROJECTS - Spring Boot - Learn

내가 사용하는 Spring Boot 를 찾은 후 Reference Doc. 으로 들어간다.

Dependency Version 으로 들어가서

찾기(command + F 윈도우는 ctrl + F )를 통해 org.hibernate 혹은 hibernate-entitymanager 를 검색해서 버전을 찾는다.

필자는 Spring Boot 2.5.5 버전을 사용하므로 5.4.32.Final 버전을 사용했다

H2는 위에서 기억한 버전을 입력한다. (필자는 1.4.200버전을 사용했다.)

## 1.2.3. persistence.xml

src/main/resources/META-INF/persistence.xml

위 디렉토리에 맞게 persistence.xml을 만든다. (위치가 중요)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<persistence version="2.2"
    xmlns="http://xmlns.jcp.org/xml/ns/persistence" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/persistence http://xmlns.jcp.org/xml/ns/persistence/persistence_2_2.xsd">
    <persistence-unit name="hello">
        <properties>
            <!-- 필수 속성 -->
            <property name="javax.persistence.jdbc.driver" value="org.h2.Driver"/>
            <property name="javax.persistence.jdbc.user" value="sa"/>
            <property name="javax.persistence.jdbc.password" value=""/>
            <property name="javax.persistence.jdbc.url" value="jdbc:h2:tcp://localhost/~/test"/>
            <property name="hibernate.dialect" value="org.hibernate.dialect.H2Dialect"/>
            <!-- 옵션 -->
            <property name="hibernate.show_sql" value="true"/>
            <property name="hibernate.format_sql" value="true"/>
            <property name="hibernate.use_sql_comments" value="true"/>
            <!--<property name="hibernate.hbm2ddl.auto" value="create" />-->
        </properties>
    </persistence-unit>
</persistence>
```

필수속성은 jdbc 속성을 주입해줘야한다.

JPA는 특정 데이터베이스에 종속되면 안되지만 SQL 문법과 함수가 조금씩 다른것을 호환하기 위해 hibernate.dialect 속성을 이용하여 바꾼다.
